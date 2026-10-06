# Implementation Plan: Add API Rate Limiting Headers (X-RateLimit-Remaining)

## Diagnosis

**Root Cause:** The `safety/rate_limiter.py` module defines a fully functional `RateLimiter.check_rate_limit()` method that enforces rate limits and tracks request counts, but **no middleware in the API layer invokes it**. The API currently registers only `RequestIDMiddleware` (as seen in `api/main.py:52`), leaving rate limiting logic disconnected from the request/response cycle. Consequently:

1. No middleware calls `check_rate_limit()` to validate incoming requests against rate limit thresholds
2. Response headers `X-RateLimit-Limit` and `X-RateLimit-Remaining` are never set
3. The API never returns HTTP 429 (Too Many Requests) when limits are exceeded
4. All requests return HTTP 200, regardless of rate limit state

**Evidence:** Grep of `api/` directory shows zero references to `check_rate_limit` or `RateLimiter`, and curl testing confirms the missing headers and absent 429 responses.

---

## Scope

### In-Scope (Must Do)

- **api/middleware/** — Create new `rate_limiter.py` middleware file modeled on `api/middleware/request_id.py`
- **api/main.py** — Register the rate limiter middleware after or alongside `RequestIDMiddleware` (around line 52)
- **Integration point** — Wire `RateLimiter` instance (from `safety/rate_limiter.py`) into the middleware, passing a Redis client if needed
- **Response headers** — Set `X-RateLimit-Limit` and `X-RateLimit-Remaining` on outgoing responses
- **429 handling** — Return HTTP 429 status when `check_rate_limit()` signals the limit is exceeded

### Out-of-Scope (Do Not Do)

- Modifying `safety/rate_limiter.py` — it is already correct and functional
- Changing the rate limit algorithm, thresholds, or Redis key structure
- Modifying authentication or authorization logic
- Database schema changes or new data models
- Creating a separate admin API for rate limit configuration

---

## Approach & Executability

### Step 1: Examine the Reference Implementation

Read `api/middleware/request_id.py` to understand:

- How middleware is structured in this project (class-based vs. function-based)
- How it accesses request context and modifies response headers
- Decorator or registration pattern used

### Step 2: Create Rate Limiter Middleware

In `api/middleware/rate_limiter.py`:

1. Import `check_rate_limit()` and `RateLimiter` from `safety.rate_limiter`
2. Create a middleware class/function that:
   - Extracts the client identifier (IP address or user_id if authenticated)
   - Calls `check_rate_limit(identifier)` **before** the route handler runs
   - If limit exceeded → immediately return `HTTPException(status_code=429, detail="Too Many Requests")`
   - If within limit → let request proceed
   - On response, inject headers:
     - `X-RateLimit-Limit` → the configured limit (e.g., "100")
     - `X-RateLimit-Remaining` → remaining count from `check_rate_limit()` response or state

### Step 3: Register Middleware in api/main.py

1. Locate the line `app.add_middleware(RequestIDMiddleware)` (~line 52)
2. Add below it: `app.add_middleware(RateLimiterMiddleware)` or equivalent
3. Ensure Redis client is initialized before middleware registration (if needed by `RateLimiter`)

### Step 4: Validate Integration

- Middleware must run on **every request** (before route handlers)
- Rate limit state must persist across requests (via Redis)
- Headers must appear on all successful responses (200, 201, etc.)
- 429 must return before the route handler is invoked

---

## Test Plan

Run these commands from a terminal with the API running locally (`http://localhost:8000`):

### Test 1: Verify Headers on Single Request

```bash
curl -i http://localhost:8000/
```

**Expected:**

```
HTTP/1.1 200 OK
x-ratelimit-limit: 100
x-ratelimit-remaining: 99
x-request-id: <uuid>
```

**Actual before fix:** Headers absent  
**Actual after fix:** Headers present with counts decreasing

### Test 2: Confirm Remaining Count Decreases Across Requests

```bash
for i in {1..5}; do
  echo "=== Request $i ==="
  curl -s http://localhost:8000/ | grep -E "message|version"
  curl -s -I http://localhost:8000/ | grep "x-ratelimit-remaining"
done
```

**Expected:** Remaining count decreases: 100 → 99 → 98 → 97 → 96

### Test 3: Trigger 429 Too Many Requests

```bash
# Spam 101+ requests (assuming limit is 100)
for i in {1..105}; do
  status=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8000/)
  echo "Request $i: $status"
done | tail -20
```

**Expected:** First ~100 return 200, then requests 101+ return 429

### Test 4: Verify 429 Response Structure

```bash
# After rate limit is hit, one more request
curl -i http://localhost:8000/
```

**Expected:**

```
HTTP/1.1 429 Too Many Requests
x-ratelimit-limit: 100
x-ratelimit-remaining: 0
```

### Test 5: Confirm Window Reset (if time-based)

```bash
# Hit limit, wait for window to reset (e.g., 60 seconds), then retry
for i in {1..105}; do curl -s http://localhost:8000/ > /dev/null; done
echo "Sleeping 65 seconds..."
sleep 65
curl -i http://localhost:8000/
```

**Expected:** After reset window, response returns 200 with fresh limit header

---

## Risks & Unknowns

### Known Risks

1. **Redis dependency:** If Redis is unavailable or the client is not initialized before middleware runs, requests may fail or bypass rate limiting. Ensure Redis is started and connection is tested.
2. **Identifier extraction:** If client identification (IP or user_id) is fragile, attackers may spoof identities. Verify the method matches the issue requirements (IP-based vs. user-based).
3. **Performance:** Middleware must be fast; if `check_rate_limit()` makes a blocking Redis call on every request, high-volume APIs may see latency. Benchmark after integration.
4. **Window overlap:** If multiple instances of the API run, ensure Redis keys are consistent across all instances (no local state).

### Unknowns

1. **Configured limit value:** The reproduction assumes limit is 100, but the actual threshold may differ. Check `safety/rate_limiter.py` for the default or environment variable.
2. **Time window duration:** How long before counts reset? The reproduction does not test this explicitly.
3. **Identifier method:** Does the issue require IP-based, user_id-based, or API-key-based rate limiting? The reproduction does not clarify; assume IP unless stated otherwise.
4. **Error message format:** What should the 429 response body contain? Copy the format from an existing error response in the API (e.g., `{"detail": "Too Many Requests"}` or a custom structure).

---

## Deviations

### What Went Well

- Middleware correctly injected `X-RateLimit-Limit` and `X-RateLimit-Remaining` headers on normal requests
- Headers counted down as expected across multiple requests within the window
- IP extraction and client identification working correctly (supports proxied requests via `X-Forwarded-For`)
- Graceful degradation when Redis unavailable (fail-open design)
- All existing unit tests pass without modification

### Deviation/Hiccup Encountered

Initial implementation raised an `HTTPException` when rate limit was exceeded. While this technically returns HTTP 429, unhandled exceptions in middleware can propagate unexpectedly and bypass middleware-level error handlers, potentially causing HTTP 500 errors in some FastAPI configurations.

### Resolution

Refactored the middleware to explicitly catch rate-limit threshold breaches and return a structured `JSONResponse` with:

- `status_code`: 429
- `content`: `{"detail": "Rate limit exceeded"}`
- `headers`: `X-RateLimit-Limit` and `X-RateLimit-Remaining` (set to "0" on limit exceeded)

This ensures the response bypasses exception handling and is consistent with FastAPI's HTTP exception response format.

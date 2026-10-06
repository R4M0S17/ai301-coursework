# Plan Comment for GitHub Issue #34

## To Post In Reply

---

Thanks for the context. I've completed a detailed reproduction and root-cause analysis. Here's my implementation plan:

### Root Cause

The `safety/rate_limiter.py` module is fully implemented but **disconnected from the API layer**. No middleware in `api/main.py` or `api/middleware/` calls `check_rate_limit()`, so responses never include `X-RateLimit-Limit` or `X-RateLimit-Remaining` headers, and the API never returns 429 when limits are exceeded.

### Implementation Approach

I will:

1. **Create `api/middleware/rate_limiter.py`** — a new middleware class modeled on `api/middleware/request_id.py` that:
   - Extracts the client identifier (IP address)
   - Calls `check_rate_limit()` before the route handler runs
   - Returns HTTP 429 if the limit is exceeded
   - Sets `X-RateLimit-Limit` and `X-RateLimit-Remaining` headers on successful responses

2. **Register the middleware in `api/main.py`** — add `app.add_middleware(RateLimiterMiddleware)` near line 52, after `RequestIDMiddleware`

3. **Verify with curl tests** — confirm headers appear on normal requests, counts decrease across requests, and 429 is returned after the limit is hit

### Test Plan

I'll run these before posting the PR:
- Single request returns 200 with rate limit headers
- Multiple requests show decreasing `X-RateLimit-Remaining`
- Request 101+ returns 429 (assuming limit is 100)
- Headers persist on all responses

### Files Affected

- `api/middleware/rate_limiter.py` — new file
- `api/main.py` — add middleware registration

**AI Disclosure:** This plan was drafted with assistance from Claude (Anthropic's AI assistant) to structure the approach and ensure completeness against the reproduction evidence.

I'll post the full implementation once I've coded and tested locally. Let me know if you'd like me to adjust the scope or approach.

---

## Notes for Posting

- **When to post:** After you've read the plan.md and verified it aligns with your understanding of the issue
- **Tone:** Respectful, specific, referencing the reproduction evidence
- **Scope:** Clear about what will and won't be changed
- **AI disclosure:** Included per evidence guide § Comms (transparency in educational context)
- **Next steps:** Indicates you'll report back with the implementation once tested


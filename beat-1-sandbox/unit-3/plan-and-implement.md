# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

R4M0S17

**Plan comment**

Link: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/34#issuecomment-6008586517

text: Root Cause
The safety/rate_limiter.py module is fully implemented but disconnected from the API layer. No middleware in api/main.py or api/middleware/ calls check_rate_limit(), so responses never include X-RateLimit-Limit or X-RateLimit-Remaining headers, and the API never returns 429 when limits are exceeded.

Implementation Approach
I will:

Create api/middleware/rate_limiter.py — a new middleware class modeled on api/middleware/request_id.py that:

Extracts the client identifier (IP address)
Calls check_rate_limit() before the route handler runs
Returns HTTP 429 if the limit is exceeded
Sets X-RateLimit-Limit and X-RateLimit-Remaining headers on successful responses
Register the middleware in api/main.py — add app.add_middleware(RateLimiterMiddleware) near line 52, after RequestIDMiddleware

Verify with curl tests — confirm headers appear on normal requests, counts decrease across requests, and 429 is returned after the limit is hit

Test Plan
I'll run these before posting the PR:

Single request returns 200 with rate limit headers
Multiple requests show decreasing X-RateLimit-Remaining
Request 101+ returns 429 (assuming limit is 100)
Headers persist on all responses
Files Affected
api/middleware/rate_limiter.py — new file
api/main.py — add middleware registration
AI Disclosure: This plan was drafted with assistance from Claude (Anthropic's AI assistant) to structure the approach and ensure completeness against the reproduction evidence.

I'll post the full implementation once I've coded and tested locally. Let me know if you'd like me to adjust the scope or approach.

---

## Your branch

**Branch**

fix/34-api-rate-limiter-headers

**Evidence**

Before:
(.venv) mb@MacBook-Pro-de-M pathreview-ai301-fa26-s1 % curl -i http://localhost:8000/
HTTP/1.1 200 OK
date: Sat, 26 Sep 2026 19:13:42 GMT
server: uvicorn
content-length: 57
content-type: application/json
vary: Origin
x-request-id: 4703ea2c-306a-4077-89ad-88fd25ad7abb

{"message":"PathReview API is running","version":"1.0.0"}

After:
(.venv) mb@MacBook-Pro-de-M pathreview-ai301-fa26-s1 % curl -i http://localhost:8000/
HTTP/1.1 200 OK
date: Tue, 06 Oct 2026 16:23:26 GMT
server: uvicorn
content-length: 57
content-type: application/json
vary: Origin
x-request-id: 23d1228f-ceca-4a4a-a937-0a169f48cb23
x-ratelimit-limit: 60
x-ratelimit-remaining: 58

{"message":"PathReview API is running","version":"1.0.0"}

(.venv) mb@MacBook-Pro-de-M pathreview-ai301-fa26-s1 % for i in {1..65}; do code=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8000/); echo "Request $i:$code"; done | tail -15
Request 51:200
Request 52:200
Request 53:200
Request 54:200
Request 55:200
Request 56:200
Request 57:200
Request 58:200
Request 59:429
Request 60:429
Request 61:429
Request 62:429
Request 63:429
Request 64:429
Request 65:429

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Initial run: agreement 17/20, failed category floor for thread-convention (1/2), misgraded pkg-14 as reject.

Revision 1 (added Thread & Convention Compliance check, adjusted Scope check): agreement 19/20, thread-convention improved to 1/2 but pkg-20 still graded as accept.

**Package analysis**

Package: pkg-14 (zellij-org/zellij#5174)

My rubric verdict: accept

Gold label: accept (clear-accept category)

Why: The plan proposes a bounded fix to the reattach handshake in the Unix client path. It explicitly names files (`zellij-server` client connection handling and `zellij-client` terminal query issuance) and defers the untestable Windows variant with a clear reason ("untestable on Linux; will flag the shared fix site for someone with a Windows setup to verify"). The initial run incorrectly rejected this because the Scope & File Boundaries check was too strict about deferrals. After adjusting the check to allow valid scope deferrals that are named with reasons, the rubric correctly recognized that deferring an untestable scenario is a reasonable boundary, not scope creep.

**Check rationale**

Check: "Thread & Convention Compliance | The plan comment read against thread highlights and repo facts | The comment engages explicit maintainer direction in the thread; if the repo facts state an AI-disclosure requirement, the comment includes a clear disclosure naming the tool and extent; the approach respects the repo's constraints | required"

Why: The initial rubric had no way to catch plans that ignored maintainer feedback or violated repo policies. This check was added because two gold-label rejects (pkg-04 and pkg-20) failed on thread and convention walls: pkg-04 ignored the maintainer's identified code location and proposed a workaround instead, and pkg-20 failed to disclose AI assistance when the repo's policy requires it. Making this check required (not preferred) ensures plans cannot pass by ignoring the maintainer's explicit direction or repo requirements. The pass condition is strict about AI disclosure: if the repo facts mention any disclosure requirement, the comment must include it or the check fails automatically.

**Trade-offs**

The Thread & Convention Compliance check gives up simplicity: it requires reading repo facts and thread highlights, not just the plan itself. This catches genuine failures (maintainers ignored, required disclosures missing) but makes the rubric context-dependent; a plan that looks good in isolation might fail if the repo has special requirements.

The Scope & File Boundaries check allows valid deferrals, which means it will not catch every scope creep that touches "unrelated" code. For example, a plan that defers the Windows variant with a reason passes; a plan that rewrites unrelated code and calls it "future work" also passes by the same rule. The check assumes the contributor is honest about what is deferred and why. This trade-off was accepted: the four scope-creep packages in the eval (pkg-06, pkg-12, pkg-15, pkg-19) all failed because they bundled unnecessary work, not because they deferred something testable.

The checks do not grade plan quality beyond the rubric's scope (code style, performance, elegance) and do not re-run tests or build the proposed change. They verify the plan is grounded, bounded, and testable—not that the execution will succeed.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

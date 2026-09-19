# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/34

**Verdict output**

Summary of Findings
All three issues are accepted — they pass all required checks. Here's the ranking for first-issue suitability:

1. Issue #34 (Top fit for first issue)

✓ maintainer-active: Last push 2026-09-16, within 90 days
✓ no-assignee: No assignees, no PRs, no claim comments
✓ scope-fits: Clear bounded task — add rate-limiting middleware (3–5 hours)
✓ language: Python (standard language)
✓ policy: No CONTRIBUTING.md; silence passes
Labels: enhancement, api, tier-2 (intermediate) 2. Issue #39 (Second choice — higher complexity)

✓ maintainer-active: Last push 2026-09-16, issue updated 2026-09-16
✓ no-assignee: No assignees, no PRs, no claim comments
✓ scope-fits: Clear scope — add progress bar, 5–8 hours
✓ language: JavaScript/Python (standard languages)
✓ policy: No CONTRIBUTING.md; silence passes
Labels: bug, frontend, tier-3 (advanced) — more complex than #34 3. Issue #40 (Third choice — broadest scope)

✓ maintainer-active: Last push 2026-09-16, within 90 days
✓ no-assignee: No assignees, no PRs, no claim comments
✓ scope-fits: Feature with link expiration logic, 5–8 hours
✓ language: JavaScript/Python (standard languages)
✓ policy: No CONTRIBUTING.md; silence passes
Labels: enhancement, frontend, tier-2 (intermediate)
Ranking rationale: Issue #34 has the shortest estimated effort (3–5 hours), smallest surface area (API middleware only), and tier-2 difficulty. Issues #39 and #40 both span frontend and backend (higher coordination) and take longer. Issue #39 is tier-3 (advanced), making #40 the safer second choice.

```
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/34",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Last push 2026-09-16 (3 days ago), within 90-day window; recent commits on default branch"},
      {"name": "no-assignee", "grade": "pass", "evidence": "assignees: [], no linked PRs, no comments with claim language"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Clear task: add rate-limiting middleware calling existing rate_limiter.py, set headers, estimated 3–5 hours"},
      {"name": "language", "grade": "pass", "evidence": "Python (FastAPI backend) — standard programming language"},
      {"name": "policy", "grade": "pass", "evidence": "No CONTRIBUTING.md or AI-use prohibition found; silence passes"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/39",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Last push 2026-09-16, issue updated 2026-09-16 (6 days ago); within 90-day window"},
      {"name": "no-assignee", "grade": "pass", "evidence": "assignees: [], no linked PRs, no comments"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Clear scope: implement real progress bar for long-running reviews using progress_pct field, estimated 5–8 hours"},
      {"name": "language", "grade": "pass", "evidence": "JavaScript/TypeScript (React) and Python (models, services) — standard languages"},
      {"name": "policy", "grade": "pass", "evidence": "No CONTRIBUTING.md or explicit prohibition found; silence passes"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/40",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Last push 2026-09-16, within 90 days; repository shows recent activity"},
      {"name": "no-assignee", "grade": "pass", "evidence": "assignees: [], no linked PRs, no comments"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Specific feature request: add shareable read-only link with 30-day expiration, estimated 5–8 hours"},
      {"name": "language", "grade": "pass", "evidence": "JavaScript/TypeScript (React) and Python (backend services) — standard languages"},
      {"name": "policy", "grade": "pass", "evidence": "No CONTRIBUTING.md or AI-use prohibition; silence passes"}
    ],
    "verdict": "accept"
  }
]

```

---

## Eval iterations

**Run history**

run 1: categories: claimed 4/4 clear-accept 1/8 dead-repo 3/3 policy 0/1 scope 4/4
agreement: 12/20 scored items (bar: 18/20: below the bar; category floor unmet: no match in policy)

run 2: categories: claimed 2/4 clear-accept 3/8 dead-repo 3/3 policy 0/1 scope 3/4
agreement: 11/20 scored items (bar: 18/20: below the bar; category floor unmet: no match in policy)

run 3: categories: claimed 2/4 clear-accept 6/8 dead-repo 3/3 policy 1/1 scope 2/4
agreement: 14/20 scored items (bar: 18/20: below the bar)

run 4: categories: claimed 3/4 clear-accept 6/8 dead-repo 3/3 policy 1/1 scope 3/4
agreement: 16/20 scored items (bar: 18/20: below the bar)

run 5: categories: claimed 4/4 clear-accept 7/8 dead-repo 3/3 policy 1/1 scope 3/4
agreement: 18/20 scored items (bar: 18/20: PASS)

**Issue analysis**

Issue: issue-19
Rubric decision: reject
Gold label: accept
Reasoning: My rubric accepted this issue, but the gold label is reject. It failed the scope check (failed: scope-fits) because the changes requested fell outside the allowed project scope defined in the guidelines.

**Check rationale**

"language | repo_facts (repository language) / Issue body | Issue uses a standard, widely-used language or stack (e.g., Python, JavaScript, TypeScript, Java, C, C++, Go, Rust, HTML/CSS, Shell, SQL). | required"

Reasoning:
I included this check to ensure I only select issues written in primary, standard programming languages that I can comfortably work with. This prevents selecting candidate issues that depend on obscure or niche stacks that could block progress during implementation.

**Trade-offs**

This check accepts that it will miss valid first-issue contributions that rely on less common domain-specific languages, specialized scripting formats, or repos where code is embedded purely in configuration files (like YAML pipelines or shell-only tools), treating them as invalid even if the task itself might be simple.

---

## Selection rationale

**Selection rationale**

1. Fit to interests and time: Issue #34 focuses on adding rate-limiting middleware in Python (FastAPI). Since I have experience in Python and backend development, this aligns with my skills and is estimated at 3–5 hours, fitting comfortably within my available schedule.

2. What the verdict identified vs. manual weighing: The rubric correctly verified that the maintainer is active, the issue has no assignees, and the task has a well-defined scope. Manually, I weighed the fact that working on a pure backend API middleware avoids frontend coordination overhead, making it a cleaner and safer first issue.

3. Anticipated difficulty in claiming: The main anticipated difficulty will be ensuring the middleware integrates properly with existing FastAPI endpoints without breaking edge cases, and coordinating the claim process following the repo's specific contribution guidelines in Unit 2.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

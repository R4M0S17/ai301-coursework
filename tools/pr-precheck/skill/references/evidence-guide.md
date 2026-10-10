# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

**Where the evidence lives:**

*In eval bundles:*
- Plan-context block: the plan's stated `scope` (which files/areas are in bounds) and `boundary` (what changes are not included)
- Deviation notes: explicit acknowledgment of any work that falls outside the stated scope
- Candidate diff: the actual file changes that occurred

*In live PRs:*
- `plan.md` in the working copy: stated scope and boundary
- Branch diff (via `git diff main...HEAD`): what actually changed
- PR description: any deviation notes or scope clarifications

**What good looks like:**
Every changed file falls within the plan's stated scope, or there is an explicit deviation note explaining why it doesn't. No file changes occur that contradict the boundary statement (e.g., "only modifying tests" but production code is also changed without noting it). The PR description's claims about scope match what the diff shows — if it claims a refactor-only change, the diff contains only refactoring; if it claims a bug fix, the fix is present and visible.

Silent drift occurs when: changed files exceed the scope with no note, the diff contradicts the plan's boundary, or the description claims one scope but the diff shows a different one.

## Test evidence (harness category: not-tested)

**Where the evidence lives:**

*In eval bundles:*
- Test-evidence section: before/after behavior, test steps run, and reproduction case outcome
- Test plan (from plan-context): what tests were proposed as proof
- Repo-facts block: what the repository's test suite or CI checks require

*In live PRs:*
- Captured test output (in description or a comment): the output of running tests or the reproduction steps
- CI/check results: visible status from the repository's own test suite or GitHub Actions
- PR description: description of what was tested and how

**What good looks like:**
The test evidence names a specific observable behavior (not just "tests pass"), states the expected-after condition clearly, and shows the test result (pass/fail with output, or before/after screenshots for UI changes). Test steps are repeatable: someone else can follow them and see the same result. CI checks pass (or are documented as inapplicable). The evidence matches the plan's stated test plan — if the plan said "add unit tests for X," the evidence shows those tests passing.

Insufficient evidence: claiming "tested" with no steps, CI failures without explanation, before/after that don't show what changed, or test results that don't address the plan's test plan.

## Diff quality (harness category: unreviewable)

**Where the evidence lives:**

*In eval bundles:*
- Candidate diff (unified format): the line-by-line changes
- Commit list: the series of commits in the PR with their messages

*In live PRs:*
- Branch diff: `git diff main...HEAD` or the PR's Files tab on GitHub
- Commit history: the commits reachable from the branch that aren't on main

**What good looks like:**
The fix or change itself is visible in the diff — the reviewer can see why the problem is solved by reading the code. Only related changes are present; no unrelated reformatting, dependency updates, or drive-by edits riding along. Commits are logical and well-described: each commit has a clear message, and the series tells a coherent story. No debris: no debug print statements, commented-out code blocks, `console.log` left in, or commented-out TODO notes that went obsolete.

Unreviewable diffs: changes so tangled with unrelated edits that the fix is hard to spot, commits with generic messages like "fix" or "update," trailing debug code, or formatting churn mixed with functional changes.

## Standards and comms (harness category: standards-wall)

**Where the evidence lives:**

*In eval bundles:*
- Repo-facts block: the repository's PR template sections (from GitHub, GitLab, or repo root), contributing guidelines, and any stated policy on AI-use disclosure
- Candidate PR description: the title and body text

*In live PRs:*
- `.github/pull_request_template.md` or `CONTRIBUTING.md` in the repo: template structure and required sections
- PR title and description (on GitHub or in draft): what the author actually wrote
- Thread/comments: whether explicit maintainer direction was engaged

**What good looks like:**
Required sections from the PR template are filled with real, specific content—not boilerplate, not placeholders left unchanged, and not "N/A" when the section applies. AI-use disclosure is present if the repository's policy requires it (stated in template or contributing guide). Commit messages follow the repository's convention (e.g., "fix: ...," "feat: ...," or prose sentences, depending on the repo's stated style). If a maintainer or the template explicitly asks for something—like a test plan, a link to an issue, or a risk assessment—it is there and addressed.

Standards-wall failures: leaving template sections as-is with default text, omitting required disclosures, using commit messages that don't match the repo's convention, or ignoring an explicit maintainer ask in the thread.

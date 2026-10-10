---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: **is this pull request
ready to submit?**

A PR package is a candidate pull request: its title, description,
commits, diff, and test evidence. You read that package against the
plan it claims to implement and against the issue that plan belongs
to. Grade one package per run, and only the package in front of you.

Do not answer any other question. Do not judge whether the work is
good in general, whether a maintainer will merge it, or what happens
after it is opened. Do not grade another PR, even one on the same
issue.

This tool is read-only. It does not push, open a PR, comment on the
issue, or edit any file in the working copy.

## Inputs and modes

Pick the mode from how you were invoked. A bundle file path means
eval mode. No bundle means live mode.

**Live mode** checks the student's own submission before it goes out.
Read these inputs:

- `plan.md`: the posted plan, including its deviation notes. A
  deviation counts as part of the plan only when `plan.md` records it.
  A change that exists only in the diff is not a recorded deviation.
- `git diff main...HEAD`, run from the working copy. Use three dots.
  This is everything the branch changes relative to the default
  branch. Also run `git log main..HEAD --oneline` for the commit list.
- `pr_draft.md`: the draft PR title and description.
- `test_evidence.md`: the captured output of the reproduction and of
  the repo's own checks.
- The issue thread, read with `gh issue view <number> --comments` or
  from the issue's web page. It carries the maintainer direction.
- The repo's PR template.
- `docs/CONTRIBUTING.md` from the working copy.

A house-chain student reads the house plan and the house repro pack
that the instructor routed in place of their own `plan.md` and repro
comment. The same checks grade the same things there.

**Eval mode** treats the bundle as the whole world. Read the bundle
file once, and take every fact from its text. Do not run git, call
`gh`, fetch the web, or read any other file. The bundle has these
sections:

- `Repo facts`: the PR template's asks and the contribution policy.
- `Issue` and `Thread highlights`: the issue and its discussion.
- `Plan context`: the accepted plan, its repro evidence, its scope,
  and its test plan.
- `Candidate PR`: the title, description, commits, diff, and test
  evidence.

If a section is missing or empty, every check that needs it grades
`unclear`. Eval mode always grades every check against the full
verdict rule. It never reads `scope.md` or `voice-guide.md`.

## The scope seam (live mode only)

Read `scope.md` before any other input.

- Find the `Repo:` line. If it still holds a placeholder such as
  `<ORG>/<PATH-REVIEW-REPO>`, or any other bracketed placeholder, or
  it is empty, stop without grading. Tell the student to fill the
  `Repo:` line in `scope.md` with their section's Path Review repo.
  That repo is named in the install step of this unit's activity page.
  If it is not there, they should ask their instructor. Never guess a
  scope.
- Confirm the PR targets the scoped repo, using the upstream remote
  or the PR URL if one exists. If it targets any other repo, refuse to
  grade and name the repo that is in scope.
- Apply the house rules in `scope.md` as part of the bar. The PR must
  come from the fork's `fix/<issue-number>-<slug>` branch. It must
  implement one plan on one issue. The PR template must be used, with
  every section filled and the disclosure section filled where it
  applies. A classmate's PR on the same issue does not block this one.
- Do not look for evidence outside the scoped repo.

## The voice seam (live mode only)

Read `voice-guide.md` after the scope check. It holds the student's
own rules for outgoing text. Hold the draft title and description in
`pr_draft.md` against those rules. For each rule the draft breaks,
quote the rule and the drafted words in the summary.

A voice finding never changes the verdict by itself. It changes the
verdict only when a rubric check reads the voice guide. Eval mode
ignores `voice-guide.md` entirely.

## Component reads

Read the components in this order, after the scope and voice steps
in live mode:

1. `rubric.md`: the checks, their evidence sources, their pass
   conditions, their weights, and the verdict rule.
2. `references/evidence-guide.md`: where each evidence family lives in
   a PR package, and what good looks like there.
3. `procedure.md`: the steps. Execute it as written, in the order it
   gives.

Refuse to grade, and say so, when either component has no content:

- If `rubric.md` has no filled check rows, or no verdict rule, stop.
  Tell the student this tool cannot grade without a rubric.
- If `procedure.md` has no filled steps, stop. Tell the student this
  tool cannot grade without a procedure.

These refusals apply in both modes.

Refuse in live mode when a required input is missing:

- No `plan.md`, or no `pr_draft.md`: stop and name the missing file.
- An empty `git diff main...HEAD`: stop, because there is nothing to
  submit.
- No local `main` branch: stop and say so. Do not guess another base.

A missing `test_evidence.md` does not stop the run. Grade the tests
checks as `unclear`, and the verdict rule decides what that means.

When `procedure.md` is silent on a step, do not invent that step. Name
the gap in the summary. Continue with the checks that do not depend on
it. A check that does depend on the missing step grades `unclear`.

When `evidence-guide.md` does not locate the evidence a check names,
grade that check `unclear` and say which evidence was not found.

## Verdict and output

The verdict is binary. `accept` means ready to submit. `reject` means
hold. There is no third verdict. Put reservations in the evidence
lines, not in the verdict.

Assemble the verdict with the verdict rule in `rubric.md`, applied to
the grades you recorded. Your procedure governs how the per-check
grades become the verdict.

Write the reply in this order, and end it with nothing after the JSON
block:

1. A short readable summary, one line per check: its name, its grade,
   and the fact or quote that decided it.
2. In live mode, the voice notes, quoting each broken rule.
3. Any procedure gaps, named as gaps.
4. The verdict, plus the name of the check the verdict turned on. The
   procedure says which check that is when several fail.
5. A fenced JSON block, valid and last.

The JSON block must follow this schema exactly. Do not alter, extend,
or reorder it. Its `checks` array lists one entry per rubric check, in
rubric order, with each name exactly as `rubric.md` gives it.

- `item`: the PR URL. In live mode, if no PR is open yet, use
  `<scope repo>:<branch>`. In eval mode, use the bundle id.
- `evidence`: one line naming the fact or quote that decided the grade.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Name the fact or quote that decided every grade.
  "Looks fine" is not evidence.
- **Grade the thing, not the polish.** Read the diff against the plan,
  the test evidence against the plan's test plan, and the description
  against the diff. Do not grade length, commit count, or formatting.
- **Disclosure is not a failure.** A PR can honestly disclose a
  shortfall and still be ready. A description that claims something
  the diff does not contain is silent drift, and it fails.
- **Deviations count only when recorded.** A deviation written into
  `plan.md` is part of the plan and is graded as one. A deviation that
  exists only in the diff fails the plan-fidelity check.
- **The rubric decides, not you.** A check that passes by its stated
  condition passes, even if it feels wrong. You may note the tension
  in the summary. The fix belongs in the rubric.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written. Report its gaps instead of covering them.
- **Unclear defaults to fail.** Treat `unclear` as the verdict rule
  directs. Where the rule is silent, an unclear grade counts as a
  fail. A PR you cannot verify from the package is not ready to
  submit.
- **Stay inside the field of view.** In live mode, grade only the
  scoped repo's PR. In eval mode, use only the bundle.

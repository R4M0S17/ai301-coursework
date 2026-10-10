# Procedure: how this tool grades a PR package

This procedure runs after the scope and voice steps in `SKILL.md`. In
live mode, the scope check has already passed, and `voice-guide.md`
has already been read. If either has not happened, stop and go back
to `SKILL.md`. The check names below are the exact names in
`rubric.md`. Copy them into your notes as written.

## Read order

Read the package in this order. Keep a notes file of quotes with
their sources as you go. Each note carries its source: a file and
line, or a bundle section.

1. **Repo standards.** Live: `docs/CONTRIBUTING.md` and the repo's PR
   template. Eval: `Repo facts`. Note these asks, each with its
   source: the required check commands, the changelog or release-note
   requirement, the type-annotation rule, the closing keyword for
   issue links, and the AI-use policy. If the repo states no AI-use
   policy, note that too. Read standards first, because every later
   check measures the PR against them.
2. **Issue thread.** Live: `gh issue view <number> --comments`. Eval:
   `Issue` and `Thread highlights`. Note each explicit instruction
   from a maintainer, with the author, the date, and the quote.
   Commenters who are not maintainers give suggestions, not direction.
   Note them, but do not treat them as instructions. Read the thread
   before the plan, so the plan can be read against what the thread
   asked for.
3. **Plan.** Live: `plan.md`. Eval: `Plan context`. Note the issue
   number, the scope list (every file and function the plan names),
   each planned change item, each recorded deviation with its reason,
   the test plan's steps and their expected-after values, and any
   dependency the plan adds. Read the plan before the diff, so the
   diff is judged against the stated scope and not the other way
   round.
4. **Diff and commits.** Live: `git diff main...HEAD --name-only`,
   then `git diff main...HEAD`, then `git log main..HEAD --oneline`
   and each commit's `git show --stat <sha>`. Eval: `Candidate PR`,
   `Commits` and `Diff`. Note each changed file, each hunk's file and
   function, and each commit's file list. Read the file list before
   the full diff, so you know the whole footprint before reading
   details.
5. **Test evidence.** Live: `test_evidence.md`. Eval: the `Test
   evidence` part of `Candidate PR`. Note each command shown, its
   branch or commit identity if the output shows one, its output, and
   its outcome. Read it after the plan, so each command can be matched
   to a test-plan step.
6. **Draft text.** Live: `pr_draft.md`. Eval: `Title` and `Description`
   in `Candidate PR`. Note the title, each section heading, and each
   factual claim in the description. Read it last, because it makes
   claims against everything read before it.

Read every part in full before grading any check. Do not begin
grading in the middle of reading.

## Evidence gathering

Each check's evidence comes from the notes you took during the reads.
The gathering moves below pair two sources and say what to record.
Gather them after the reads and before any grading.

**Silent drift: changed files stay inside the plan's scope.** List
every changed file from step 4. List every file the plan's scope names
from step 3, and every file named in a deviation note. Record each
changed file as in scope, deviated, or unaccounted.

**Silent drift: every hunk and every planned change matches.** Go hunk
by hunk through the full diff. For each hunk, record the file and
function, then the plan change item it realizes, its test, or the
deviation that covers it. A hunk that maps to none is unaccounted.
Then go the other way: for each planned change item, record the hunk
that realizes it. A planned item with no hunk is unaccounted unless a
deviation covers it.

**Silent drift: description claims match the diff.** From the
description in step 6, list each sentence that claims something
changed, something was run, or a behavior now holds. For each claim,
record the diff hunk or the evidence block that supports it, or record
that it is contradicted or absent.

**Not tested: captured proof shows the plan's test plan ran on the
branch.** For each step of the plan's test plan, find the matching
command and output in the test evidence. Record whether the output
shows the branch (a branch name, a commit SHA equal to `HEAD`, or the
bundle's stated branch), the output text, and whether the output
matches the expected-after value the plan states. Mark a block as
placeholder if it is "TODO", "paste output here", a bracketed
template, or "tests pass" with no output beneath it.

**Not tested: named tests and fixtures exist.** From the plan and the
description, list every test, fixture, and test file they name. For
each one, record whether it appears in the changed-file list or
exists in the repo at the branch. In eval mode, record whether it
appears in the diff.

**Not tested: the repo's own checks ran and passed.** From the repo
standards (step 1), list every check command that the repo names as
required. For each one, find its output in the test evidence. Record
the command, the output, and whether the output shows success: a zero
exit status, a passing count, or a clean report.

**Unreviewable: added lines hold no debris and no committed
artifacts.** Take every added line of the full diff (lines starting
with `+`, not `+++`). Scan each one for debug leftovers: print or log
calls, breakpoints, debugger statements, and `TODO` or `FIXME` text
the plan did not create. Scan for commented-out code blocks. Scan for
whitespace-only changes outside the change. From the file list, scan
for build output, lock files, IDE files, and scratch or log files.
Record each hit with its file and line. A hit that the plan names, or
that the description explains, is not debris.

**Unreviewable: commit messages name what each commit changes.** For
each commit from step 4, record its message and the files it touches.
Then compare the message's claim with that commit's own changes. If
the bundle or the repo does not let you attribute hunks to commits,
record that the attribution is not possible.

**Standards wall: PR template sections are filled with this PR's
content.** From the template in step 1, list every section heading and
any conditional-section rule. For each heading, find its text in the
description. Record whether the text is this PR's content, the
template's own placeholder or instruction text left in place, or
absent. For a conditional section, record whether it is marked not
applicable with a reason, or filled.

**Standards wall: description links the plan's issue with the required
keyword.** From the plan (step 3), take the issue number. From the
template or contribution policy, take the required closing keyword.
Search the description for that keyword and number. Record the
number found, or that none is found.

**Standards wall: CONTRIBUTING asks are met or marked not
applicable.** From `docs/CONTRIBUTING.md` and the template's checklist
(step 1), list every stated requirement that this PR could meet: a
changelog or release-note entry, type annotations on new arguments, a
required test. For each one, record whether the diff meets it, whether
the description marks it not applicable and gives a reason, or
whether it is unaddressed.

**Standards wall: AI-use disclosure is present where the policy asks
for one.** From step 1, record the policy's exact question, or that no
AI-use policy is stated. If a policy exists, find the disclosure in
the description and record the sentence that answers the policy's
question, or that none is present.

**Standards wall: explicit maintainer direction in the thread is
followed or answered.** From step 2, list every explicit maintainer
instruction. For each one, record whether the diff follows it (the
hunk), or whether the description answers it with a stated reason (the
sentence). If the thread holds none, record that.

**Comms: the title names the change the diff makes.** From step 4,
state the diff's main change in one sentence. From step 6, record the
title. Compare them, and record the terms the title uses for the
change.

Voice notes from `voice-guide.md` are gathered here as well, from the
title and description in step 6. They are reported in the summary and
never graded.

## Check execution

Execute the 14 checks in the order `rubric.md` lists them. Grade each
check independently. Do not stop early and do not skip a check because
another check failed. Every check is graded and recorded, so two
executors reach the same grades.

For each check:

1. Take the notes gathered for it. Do not re-read the whole package.
   Re-read a single source only when a note is missing or ambiguous,
   and record that you did so.
2. Apply the pass condition in `rubric.md` to those notes, literally.
   A check passes only when its stated condition holds. A feeling
   that the PR is fine does not pass a check.
3. Record the grade (`pass`, `fail`, or `unclear`) and one evidence
   line. The evidence line quotes the fact or the quote that decided
   the grade, with its source.

Three rules govern missing evidence. Apply them in this order:

- **The condition does not apply.** Grade `pass`, and write the reason
  as the evidence line. Use this for a standard the repo does not
  state (no AI-use policy, no required check, no maintainer
  instruction in the thread). A vacuous pass is not `unclear`.
- **The evidence should exist and cannot be found.** Grade `unclear`,
  and name the missing source in the evidence line. Use this when the
  template or `CONTRIBUTING.md` is present but a required section
  cannot be matched, or when a test-plan step has no output block in
  the evidence.
- **The source itself cannot be read.** Grade `unclear`, and record it
  as a procedure gap in the summary. A missing `test_evidence.md` is
  the example: the test-evidence checks grade `unclear`, and the
  verdict rule decides what that means. A missing `pr_draft.md` or
  `plan.md` does not reach this step, because `SKILL.md` refuses
  before grading.

If the procedure cannot give a step for a case you hit, do not invent
one. Record the case as a procedure gap in the summary, and grade the
check `unclear` if the gap blocks it.

Checks 1 through 3 depend on the gathered pairings and should be graded
after all the gathering for them is done. The other checks can be
graded as soon as their notes are complete.

## Verdict assembly

Assemble the verdict from the 14 recorded grades. Apply the rule in
`rubric.md`, in this order:

1. **Map `unclear` for required checks.** Each required check graded
   `unclear` counts as `fail` for the verdict. Keep its recorded grade
   `unclear` in the output; only the verdict uses the mapping.
2. **Ignore preferred checks for the verdict.** Preferred checks
   (`Unreviewable: commit messages name what each commit changes` and
   `Comms: the title names the change the diff makes`) never change the
   verdict, whatever their grade. Record their grades in the output.
3. **Set the verdict.** If any required check, after mapping, is
   `fail`, the verdict is `reject`. If every required check, after
   mapping, is `pass`, the verdict is `accept`.
4. **Pick the deciding check.** The deciding check is the first
   required check in rubric order whose grade is `fail` after mapping.
   If the verdict is `accept`, the deciding check is the first required
   check in rubric order, and its evidence line is quoted as the basis
   for accepting.
5. **Quote the deciding check.** In the summary, quote the deciding
   check's evidence line with its source. If the check was graded
   `unclear`, say so, and say it was mapped to `fail`.

Write the reply as `SKILL.md` specifies. Put a readable line per check
first, in rubric order. Then the voice notes, then any procedure gaps.
Then the verdict and the deciding check. The JSON block comes last.
Its `checks` array lists each check's recorded grade as it was graded,
including `unclear`, and its `verdict` is the result of step 3.

The same grades must produce the same verdict every time. Apply the
steps in this order, with no judgment step between them, and the
result follows from the grades.

# Rubric: is this pull request ready to submit?

## Checks

Each check names its failure family in its name, so the harness can
score categories by name. The table runs in the order the verdict
rule reads it: the procedure quotes the first failing required check
in this order.

Evidence cells give the live source first, then the eval source
after the slash. Live means the working copy and GitHub. Eval means
the bundle section.

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Silent drift: changed files stay inside the plan's scope | Live: `git diff main...HEAD --name-only` read against the file list in the scope section of `plan.md`, and the deviation notes in `plan.md`. Eval: the file paths in the `Candidate PR` diff read against the scope pair in `Plan context`. | Every changed file is named in the plan's scope, or is named in a deviation note in `plan.md` (eval: in the plan context) with a reason. Pass only if no changed file fails both. A file that is in the diff but in neither place fails. | required |
| Silent drift: every hunk and every planned change matches | Live: `git diff main...HEAD` read hunk by hunk against the change items in `plan.md`. Eval: the `Diff` read against the `Plan:` paragraph in `Plan context`. | Each hunk traces to a plan change item, its test, or a recorded deviation, and each plan change item appears in the diff or in a recorded deviation. A hunk that does more than the plan says, or a plan item the diff silently omits, fails. A deviation recorded in the plan with its reason passes either way. | required |
| Silent drift: description claims match the diff | Live: every factual claim in `pr_draft.md` (files touched, behavior changed, tests added, fix described) checked against the diff and `test_evidence.md`. Eval: the `Description` read against the `Diff` and `Test evidence` in `Candidate PR`. | Every claim the description makes about what changed or what was run is present in the diff or in the captured evidence. A claim the diff contradicts or does not contain fails. A claim the description marks as not done passes if the diff agrees. | required |
| Not tested: captured proof shows the plan's test plan ran on the branch | Live: `test_evidence.md` read against the test plan in `plan.md`. Eval: the `Test evidence` section read against the `Test plan` in `Plan context`. | The evidence shows real command output from the branch, not main alone, for each step the plan's test plan names. Each step shows the expected-after value the plan states. Placeholder text ("TODO", "paste output here", "tests pass" with no output) counts as no evidence. An unrun step passes only if the description discloses it and the disclosure matches the evidence. | required |
| Not tested: named tests and fixtures exist | Live: each test, fixture, or test file that `plan.md` or `pr_draft.md` names, located in `git diff main...HEAD --name-only` or in the repo at the branch. Eval: the names in the `Plan` and `Description` read against the `Diff`. | Every named test, fixture, or test file exists at the branch. A named item that is missing and not disclosed fails. A missing item the description discloses as not done passes if the plan's test plan does not require it. | required |
| Not tested: the repo's own checks ran and passed | Live: the check or suite commands that `docs/CONTRIBUTING.md` or the PR template names, and their output in `test_evidence.md`. Eval: the checks named in `Repo facts` read against the output in `Test evidence`. | Every check the repo names as required has captured output on the branch showing success (a zero exit, a passing count, or a clean report). If the repo names no required check, this passes with the evidence line "no required check named". A required check with no output, or with failing output not disclosed, fails. | required |
| Unreviewable: added lines hold no debris and no committed artifacts | Live: the added lines of `git diff main...HEAD` (lines starting with `+`), and `--name-only` for committed build output, lock files, IDE files, and scratch files. Eval: the added lines in the `Diff`. | No added line is a debug leftover (print or log statements, breakpoints, a stray `TODO` the plan did not create), a commented-out block, or whitespace-only churn outside the change. No artifact the plan does not name is committed. Any hit that the plan names or the description explains passes. | required |
| Unreviewable: commit messages name what each commit changes | Live: `git log main..HEAD` read against each commit's diff. Eval: the `Commits` list read against the `Diff`. | Each commit message names a change that its own diff contains. A message describing a change that is not in that commit fails. | preferred |
| Standards wall: PR template sections are filled with this PR's content | Live: the PR template, read against `pr_draft.md`. Eval: the template asks in `Repo facts` read against the `Description`. | Every section the template asks for has text specific to this PR. Template placeholder text left in place fails. A section the template marks as conditional passes with "not applicable" and the reason, or with real content. | required |
| Standards wall: description links the plan's issue with the required keyword | Live: the issue number in `plan.md`, read against `pr_draft.md`. Eval: the `Issue` number in `Issue` read against the `Description`, and the closing-keyword ask in `Repo facts`. | The description references the same issue number as the plan, using the keyword the repo's template requires (for example "closes #N"). A missing reference, or a reference to another issue, fails. | required |
| Standards wall: CONTRIBUTING asks are met or marked not applicable | Live: `docs/CONTRIBUTING.md` and the PR template's checklist, read against the diff and the description. Eval: the contribution policy in `Repo facts` read against the `Candidate PR`. | Every requirement that the repo states and that this PR can meet (a changelog or release-note entry, type annotations on new arguments, a required test) is met in the diff, or the description marks it not applicable with a reason. A stated requirement that the PR can meet and does not, with no note, fails. | required |
| Standards wall: AI-use disclosure is present where the policy asks for one | Live: the repo's stated AI policy in `docs/CONTRIBUTING.md` and the disclosure section of the template, read against `pr_draft.md`. Eval: the AI policy line in `Repo facts` read against the `Description`. | If the policy asks for an AI-use disclosure, the description contains one that answers the policy's question. If the policy names no AI-use rule, this passes with the evidence line "no AI policy stated". | required |
| Standards wall: explicit maintainer direction in the thread is followed or answered | Live: the issue thread, read for explicit maintainer instructions, against the diff and the description. Eval: the `Thread highlights` read against the `Candidate PR`. | Every explicit maintainer instruction in the thread is either followed in the diff or answered in the description with a reason. If the thread holds no explicit instruction, this passes with the evidence line "no explicit maintainer direction". An ignored instruction fails. | required |
| Comms: the title names the change the diff makes | Live: `pr_draft.md` title read against the diff's main change. Eval: the `Title` in `Candidate PR` read against the `Diff`. | The title names the same change the diff makes, in the diff's own terms. A title naming a different change, or a title that names no change, fails. | preferred |

## Verdict rule

The verdict is binary. `accept` means ready to submit. `reject` means
hold.

1. **Required checks decide.** Accept only if every `required` check
   grades `pass`. Any `required` check that grades `fail` gives
   `reject`, with no exceptions.
2. **Unclear counts as fail for required checks.** A `required` check
   that grades `unclear` is treated as `fail`, and the verdict is
   `reject`. An unverifiable claim is a claim the package does not
   support.
3. **Preferred checks never change the verdict.** A `preferred` check
   may grade `pass`, `fail`, or `unclear`. The grade is recorded in
   the output and the summary, and the verdict ignores it.
4. **A vacuous pass is a pass.** Where a check's condition does not
   apply to the package (the repo states no AI policy, the thread
   holds no instruction, the repo names no required check), the check
   grades `pass` and its evidence line names the reason. It does not
   grade `unclear`. Unclear means the evidence should exist and cannot
   be found.
5. **Honest disclosure is not a failure.** A PR that discloses an
   unrun step, a partial fix, or a deviation, and whose disclosure
   matches the evidence, can grade `pass` on the check it affects. The
   verdict is `reject` only when the package claims something the
   evidence does not show, or leaves a required gap undisclosed.
6. **The deciding check.** The verdict is quoted against the first
   failing or unclear required check in table order. The procedure
   applies this rule when it writes the summary.

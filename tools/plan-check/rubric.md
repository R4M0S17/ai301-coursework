# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check                            | Evidence                                                             | Pass condition                                                                                                                                                                                 | Weight    |
| -------------------------------- | -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| Diagnosis & Evidence Alignment   | The plan's Diagnosis section read against the Repro Evidence section | The diagnosis directly explains the exact failure shown in the reproduction logs/trace without relying on unverified assumptions                                                               | required  |
| Scope & File Boundaries          | The plan's Proposed Changes section                                  | The plan explicitly names specific files/functions and limits changes to fixing the bug. Valid scope deferrals (untestable scenarios, follow-up work) are stated as deliberate exclusions with reasons | required  |
| Test Plan Verification           | The plan's Test Plan section                                         | Provides a specific test command, automated unit test, or manual reproduction step that will verify the bug is resolved                                                                         | required  |
| Thread & Convention Compliance   | The plan comment read against thread highlights and repo facts       | The comment engages explicit maintainer direction in the thread; any AI disclosure required by the repo's stated policy is included; the approach respects the repo's constraints                 | required  |
| Minimal Risk & Dependencies      | The plan's Implementation Details section                            | The proposed solution does not introduce new third-party dependencies or breaking API changes                                                                                                   | preferred |

## Verdict rule

Accept if all required checks pass; preferred checks never change the verdict; unclear counts as fail.

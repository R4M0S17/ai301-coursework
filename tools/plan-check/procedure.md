# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

Read the plan package in this order to build the correct context for grading:

1. Start with Repo Facts to understand the repo's contribution policy, AI disclosure requirements, and maintainer bandwidth notes
2. Read Thread Highlights to see if the maintainer gave explicit direction
3. Read the Repro Evidence section to understand what the actual bug behavior is
4. Read the Diagnosis section to see what root cause the plan claims
5. Review the Proposed Changes section to see which files will be modified and what is explicitly deferred
6. Read the Test Plan section to see what verification steps are proposed
7. Read the Candidate Plan Comment to assess how it engages with the thread and repo context

This order matters because you need to understand the repo's requirements and maintainer context first, then assess whether the plan respects that context, then judge whether the diagnosis matches the evidence.

## Evidence gathering

For each check, extract and record the following evidence:

**Check 1: Diagnosis and Evidence Alignment**
Pull the claimed root cause from the Diagnosis section. Extract the specific failure behavior from the Repro Evidence section (logs, traces, error messages). Record whether the diagnosis directly explains the exact failure shown without making unverified assumptions.

**Check 2: Scope and File Boundaries**
Extract the list of files and functions named in the Proposed Changes section. Record whether the plan names specific locations being altered. Check both what is explicitly in scope AND what is explicitly deferred. For scope deferrals to pass: they must be named (not just omitted), they must have reasons (e.g., "untestable on this platform", "dependent on another PR"), and they must not be arbitrary exclusions of work the issue clearly needs. Valid deferrals narrow the plan to something achievable; invalid ones dodge necessary work.

**Check 3: Test Plan Verification**
Extract the test commands, automated test code, or manual reproduction steps from the Test Plan section. Record whether these steps are specific enough to actually verify the bug is resolved.

**Check 4: Thread and Convention Compliance**
Extract the Repo Facts (contribution policy, AI disclosure requirement) and Thread Highlights (maintainer direction). Extract the Candidate Plan Comment. Before grading, first check: does the repo facts block mention any AI-use disclosure requirement? If yes, scan the plan comment for an explicit disclosure statement (must name the tool used and extent of assistance). If the repo requires disclosure and the comment lacks one, this check fails automatically—do not continue further. If disclosure is not required or is present, then record: (1) whether the comment engages explicit maintainer direction present in the thread (does it acknowledge a suggested fix site, a technical constraint, a bandwidth note?); (2) whether the approach respects the repo's stated constraints (e.g., review-bandwidth scarcity, first-contributor review flow, policy on scope). A missing required AI disclosure is a fail; all three other aspects must pass for the overall check to pass.

**Check 5: Minimal Risk and Dependencies**
Scan the Proposed Changes section for any new third party dependencies being added. Look for any breaking API changes or architectural changes that go beyond the bug fix. Record what you find.

## Check execution

Execute the checks in this order: 1, 2, 3, 4, 5. Each check is graded independently.

If evidence for a check is genuinely absent from the plan package (for example, no Test Plan section exists at all), mark that check as unclear rather than automatically failing. The executor should note what evidence was missing. Exception: Thread and Convention Compliance always has evidence (every package has repo facts and a comment), so unclear never applies; either the comment meets the requirements or it does not.

For any check whose evidence is present in the package, grade it immediately without re-reading other sections. Two executors grading the same package should reach the same grade for each check.

## Verdict assembly

Apply the verdict rule from the rubric:
- Accept if all required checks pass
- Preferred checks never change the verdict (a failed preferred check does not cause rejection)
- Unclear grades count as fail

If a plan receives a fail on any required check, the final verdict is reject. If all required checks pass, the final verdict is accept, regardless of the preferred check grades.

Quote in the output the evidence from the deciding check that caused an acceptance or rejection. For example, if a required check fails, quote the specific diagnosis text that contradicted the repro evidence.

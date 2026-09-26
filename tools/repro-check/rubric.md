# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| evidence-backed | Logs, screenshots, examples, measurements, or references in the report | The report includes concrete evidence supporting the problem | required |
| problem-specific | The reported problem description and behavior | The report identifies one specific problem and describes the concrete behavior, condition, or outcome that constitutes the problem | required |
| unfollowable-comms | The claim comment and steps in the repro report against the Environment and Steps sections of the evidence guide | Steps describe a reproducible sequence starting from a clear initial state with concrete commands; references to external artifacts (the issue's code snippet, a repo's config) are acceptable if the artifact is stable and accessible; claim comment is specific to the issue's details, not generic boilerplate; no vague instructions or missing prerequisites that would block a stranger | required |
| ai-disclosure | The repo's contribution policy (CONTRIBUTING.md or AI_POLICY.md) for any explicit AI disclosure requirement, and the claim comment for the required statement | If the repo's stated policy explicitly requires disclosure of AI usage (e.g., "all AI usage must be disclosed", "state the tool used and extent of assistance"), the claim and/or repro comment MUST include an explicit statement naming the AI tool(s) used and the nature of the assistance; absence of this required statement when the policy demands it is an automatic fail. If the policy permits AI use without disclosure or makes no mention of AI, this check passes | required |
| wrong-target | The repro report's environment (versions, branches, releases) against the issue body and repo-facts for stated target version/branch | The report targets the same version/branch as the issue specifies, or a reasonably close variant; if the issue targets "latest release" or "main branch", a reproduction on an outdated, legacy, or significantly older version (e.g., pandas 1.5.3 when the issue was filed against v3.0.5 latest) is wrong-target and must fail; minor version variations on the same release line or newer patch/minor versions are acceptable; the report must acknowledge version deltas when present | required |

## Verdict rule

All required checks must pass; ? counts as a fail.

# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

## Checks

| Check             | Evidence                                               | Pass condition                                                                                                                                   | Weight   |
| ----------------- | ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| maintainer-active | repo_facts (commits, pushes, releases, response times) | Repository has recent activity on the default branch within the last 90 days and active maintainer responses.                                    | required |
| no-assignee       | repo_facts / issue headers                             | Issue has no active assignees (`assignees` is empty, none, or `[]`). If someone is already assigned, it must fail.                               | required |
| scope-fits        | Issue body, description, and requirements              | Issue scope is clear, well-defined, and of low to medium complexity, making it manageable for a first-time contributor.                          | required |
| language          | repo_facts (repository language) / Issue body          | Issue uses a standard, widely-used language or stack (e.g., Python, JavaScript, TypeScript, Java, C, C++, Go, Rust, HTML/CSS, Shell, SQL).       | required |
| policy            | CONTRIBUTING.md, CODE_OF_CONDUCT, or repo_facts        | Repository does not explicitly prohibit AI-assisted contributions. If policies are standard, neutral, or don't mention AI, this check must PASS. | required |

## Verdict rule

Accept if and only if all five required checks pass. If any required check fails, is unclear, or cannot be verified, reject the issue.

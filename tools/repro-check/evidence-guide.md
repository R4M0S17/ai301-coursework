# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

- **Where it lives:** In an eval package, the environment record appears in the repro_report_markdown field, usually as the first section after the report title, listing versions and OS details. Cross-reference against the issue body and repo-facts to verify the target version/branch. In live mode on GitHub, the environment goes in the repro comment, typically near the top or in a structured section (the issue template may ask for it separately, e.g., `conda info` output or `starship --version`).

- **What good looks like:** The record names exact version numbers (not "latest" or "recent"), lists all relevant dependencies or tools mentioned in the issue (e.g., HTTPie 3.2.4, Python 3.12.4, multidict 6.6.0 for the httpie issue), includes the OS and architecture (macOS 14.5 arm64, Ubuntu 24.04 x86_64), and specifies the installation method when it matters (pip, Homebrew, cargo install, miniforge3). **The reported versions should reasonably match the issue's target**: if the issue targets "latest release" or "main branch" and you reproduce on a newer version (e.g., p5.js 1.11.7 when issue mentioned 1.9.4/1.10.0), state the delta ("version delta stated"). If you test on an older/legacy version when the issue targets latest/main, that is wrong-target and fails (e.g., pandas 1.5.3 against v3.0.5 latest). If unable to reproduce on the target version, explicitly acknowledge the mismatch and explain what you tried.

## Steps

- **Where it lives:** In an eval package, the steps live in the repro_report_markdown under a "Steps" section or as a code block showing the exact commands executed. In live mode, they appear as numbered steps or command blocks in the repro comment, preceded by any file preparation needed (e.g., creating a minimal input file, initializing a git repo, symlinking directories).

- **What good looks like:** The steps describe a reproducible sequence from a clear starting state; they include concrete commands and arguments, and describe file setup in enough detail that a stranger can execute them (exact flags matter, e.g., `--quiet --json`, but referencing "the issue's script" is OK if the issue has a stable, accessible script); they are deterministic and repeatable (not "try this and see what happens"). Minimal steps that describe what to create and what command to run are acceptable if the description is clear enough to follow. The steps allow a stranger to reproduce the issue from scratch without external guidance.

## Behavior shown

- **Where it lives:** In an eval package, the artifacts (command output, error messages, logs, or the comparison between current and expected behavior) appear in code blocks within the repro_report_markdown. In live mode on GitHub, the artifacts are pasted or linked in the repro comment, sometimes as screenshots, console output, or linked logs.

- **What good looks like:** The report shows the actual behavior as it occurred (the real command output, the actual error message or behavior, not a paraphrase), it includes both the expected and actual outcomes (shown side-by-side, or clearly labeled "Expected" vs "Actual"), and the artifact matches the specific behavior the issue describes, not an adjacent or similar symptom. When multiple command runs are shown (e.g., with and without a custom header), the differences are clear so the reader can see what triggers the problem.

## Honesty

- **Where it lives:** In an eval package, honesty is evident in the claim_comment_markdown (what the author claims to have done and found) read against the repro_report_markdown (what was actually tested), and in statements about environment differences, unable-to-reproduce cases, or limitations. In live mode, it is in how the student acknowledges what they tested vs. what they didn't, and whether they claim reproduction or candidly state "cannot reproduce."

- **What good looks like:** If the author reproduced the issue, they state the conditions clearly and confidently (e.g., "reproduced on HTTPie 3.2.4 with exactly one custom header"). If they cannot reproduce, they say so explicitly, name the environment they tried and why it differs from the issue's target (e.g., "cannot reproduce on Linux + zsh; issue is macOS + fish, so shell or OS may be the difference"), and explain whether the issue is still plausible or they want to investigate further anyway. The report does not claim outcomes its evidence does not support (does not say "everyone has this problem" or "the app is broken" when only the author's case is shown; does not assert root cause without evidence).

## Comms

- **Where it lives:** In an eval package, the claim_comment_markdown shows how the author addresses the issue itself, acknowledges their context (e.g., first contribution), and indicates planned next steps. In live mode, the claim and repro comments appear in the GitHub issue thread and are evaluated against the repo's CONTRIBUTING.md, issue template expectations, and any stated AI-use disclosure policy (check AI_POLICY.md or the "AI" section of CONTRIBUTING.md).

- **What good looks like:** The claim and repro comments are respectful and specific, matching the repo's tone and structure expectations (if the template asks for "Environment" vs. "Behavior", use those headings). When it is a first contribution, the author mentions it naturally and explains their planned next steps (e.g., "I'll check the apply_missing_repeated_headers() path...and report back") without over-promising or making demands. **If the repo's policy explicitly requires AI disclosure** (e.g., ghostty's "all AI usage in any form must be disclosed, stating the tool used and the extent of the assistance"), the comment must include an explicit statement like "I used ChatGPT to help draft this report" or "No AI tools were used"; absence of this statement when explicitly required is a rejection. If the policy permits AI use without mandatory disclosure or makes no mention of AI, no disclosure statement is needed. The comments avoid hyperbole ("everyone has this", "the app is broken", "highest priority") and stick to what the evidence shows.



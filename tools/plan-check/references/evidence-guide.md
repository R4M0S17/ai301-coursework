# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

**Where it lives:** The plan states the root cause in its Cause or Diagnosis section. The repro evidence section contains the specific steps and observed behavior that the cause must explain.

**What good looks like:** The diagnosis names a mechanism that directly accounts for the exact failure shown in the repro evidence. For example, if the repro shows that a color does not update after a button press, the diagnosis should explain why that update does not happen in that specific code path. The cause must reference the observed behavior (the commit color stays yellow after push) without adding speculation or unverified details.

**Bad example:** "The UI probably has a refresh bug." (Too vague, does not cite what the repro showed.)

**Good example:** "After a push started from the branch commits view, the view's model is not refreshed, so commits keep their unpushed flags. Other views refresh on push because the callback includes them, but this view is missed."

## Scope

**Where it lives:** The plan's Change or Proposed Changes section states what is in scope and what is out of scope. It explicitly names files, functions, or areas being modified and what is deliberately excluded.

**What good looks like:** The plan names the specific files or functions it will touch (for example, `pkg/gui/controllers/sync_controller.go` or the `refresh()` method in a specific class). It includes a clear out-of-scope statement. Valid out-of-scope items either rule out refactoring/cleanup work the bug fix does not require, OR they explicitly defer work with a stated reason (e.g., "Windows variant deferred—untestable on Linux", "hyperlink storage refactor left for follow-up to avoid scope creep"). A bounded change modifies only what is necessary for the bug fix; if work is deferred, the plan names it and says why.

**Bad example:** "We'll fix the refresh logic throughout the codebase." (Unbounded, vague, suggests refactoring beyond the bug fix.)

**Bad example (invalid deferral):** "We'll fix the reattach path but not the Windows variant." (Deferred without reason; sounds like dodging platform parity.)

**Good example (in-scope + out-of-scope):** "In: the push completion callback in `pkg/gui/controllers/sync_controller.go` adds the commits context to its post-push refresh scope. Out: any change to how push status is computed, or to other views' refresh behavior."

**Good example (valid deferral):** "Bounded to the reattach handshake on Unix clients. The Windows session-switch variant is explicitly deferred (untestable on Linux); I will flag the shared fix site in the PR for someone with a Windows setup to verify."

## Executability

**Where it lives:** The plan's Implementation Details or Change section describes the approach, which files will be edited, and the order of work.

**What good looks like:** A stranger could begin implementing without asking the author questions. The plan states which specific files to modify, what changes to make in each file, and in what order. For example: "first update the refresh scope in the callback, then test against the repro steps, then check force push." The plan is specific enough that someone unfamiliar with the codebase could follow the steps.

**Bad example:** "Fix the refresh issue." (No detail about how or where.)

**Good example:** "Trigger a refresh of the commits context after a successful push. The push completion callback in `sync_controller.go` will add the commits context to its post-push refresh scope."

## Test plan

**Where it lives:** The plan's Test Plan or Verification section names the test command, automated test, or manual steps that will prove the bug is fixed.

**What good looks like:** The test plan references the exact reproduction steps from the repro evidence and names what must change to demonstrate success. For example, if the repro shows that pressing a button should cause a color update, the test plan must say "at that step the color must flip without leaving the view." The plan may include multiple test scenarios if needed (like testing the same fix from different code paths).

**Bad example:** "Test that the fix works." (Too vague, does not say what to test or how.)

**Good example:** "Repro steps above; at step 3 the color must flip without leaving the view. Also check the same from the main commits panel and after a force push, since both share the callback."

## Honesty

**Where it lives:** The plan acknowledges risks, unknowns, and any deviations from the straightforward path in its Risks or Unknowns section, or woven into the plan comment.

**What good looks like:** The plan states what it does not know or is uncertain about, rather than glossing over it. For example, if modifying a callback might affect other features, the plan says so and explains why it believes the risk is acceptable. The plan does not overstate confidence in details it has not verified.

**Bad example:** "No risks. This will definitely work." (False confidence, ignores that the change affects a shared callback.)

**Good example:** "The post-push callback is shared by multiple views, so this change touches a sensitive area. However, the refresh operation is additive and safe; it will only update the commits context state, which other views already do."

## Thread & Convention Compliance

**Where it lives:** The plan comment on the issue. Read it against the repo's stated contribution policy and AI requirements (in the repo facts) and any thread highlights (explicit maintainer direction).

**What good looks like:** The comment engages explicitly named maintainer direction (for example, acknowledging a maintainer's identified culprit location, or a suggestion to use a specific approach). The comment respects the repo's stated constraints (bandwidth scarcity, review selectivity, first-contributor flow). If the repo's stated policy requires AI disclosure—any form of AI assistance must be disclosed—the comment includes a clear disclosure statement. The comment does not promise what the contributor cannot deliver, and does not ignore maintainer direction present in the thread.

**Bad example (ignores maintainer direction):** "I reproduced this on Windows. I plan to document the workaround in the README." (The maintainer already identified the code location `src/tui/light_windows.go` and posted a patched binary asking for testing; the comment ignores that explicit direction and proposes a workaround instead of engaging the fix.)

**Bad example (missing AI disclosure):** A comment from a package with a strict "all AI usage must be disclosed" policy, submitting a plan with no mention of AI assistance, when the plan clearly relied on LLM help.

**Good example (engages maintainer direction):** "I reproduced both fuzz cases on current main. My plan follows the direction proposed here: a page generation counter bumped on capacity changes, with `prev` recomputed in `Terminal.print` only when the generation moved, so the hot path stays one comparison."

**Good example (respects constraints):** "Reproduced on 0.64.1. The post-push refresh scope just misses the branch commits context; plan is a one-change fix in the sync controller's push callback plus a manual check across the two commit views and force push. Will send the PR shortly; keeping it minimal given the review-bandwidth note in CONTRIBUTING."

**Good example (includes required disclosure):** "I used Claude to help outline the diagnostic steps and format. All reproduction and testing is from my own work. I generated the [description] with Claude AI assistance, but all observations and error outputs are from my own testing."

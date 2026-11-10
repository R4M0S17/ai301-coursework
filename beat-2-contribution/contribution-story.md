# Contribution: [Issue Title]

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Javier Ramos
**GitHub Username:** R4M0S17
**Issue:** https://github.com/Zwony/ecotrace/issues/195
**Pull Request:** https://github.com/Zwony/ecotrace/pull/251

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Applied issue-scout rubric to evaluate issues for claim**

I built a rubric with five checks to qualify issues: maintainer activity (recent commits), no active assignee, scope fit (clear and bounded), language (Python), and policy (no AI restrictions). The scan revealed one issue where scope-fits was unclear because a related PR had partially solved it, leaving the remaining work contested. This taught me that some issues need maintainer clarification before they are truly ready to claim. I documented what feedback would resolve the uncertainty: a maintainer comment confirming which remaining items still matter.

**Unit 6: Validated claim draft with repro-check voice-guard**

I ran repro-check on my claim draft to verify it followed the repository's policy and had evidence backing it. The tool passed the ai-disclosure check and accepted the claim, but found two issues: the scout had conflicting verdicts in the same file (one entry said accept, another said reject), and the voice guide was still blank so no policy rules existed to check against. These gaps meant four checks could not yet be graded. I documented the conflict and noted that the next step is to write the repro-report.md file to complete the full claim package.

**Unit 7: Validated branch with ship-gate before PR submission**

I created a ship-manifest.md file defining the validation rules, then ran the AI301 ship-gate validation script (python3 ~/.claude/ship-gate/check_gate.py) to intercept my submission. The script automatically checked my branch against the manifest: running the test suite, style linters (PEP 8), and pr-precheck rules on my changes to ensure zero errors before the PR was allowed to proceed. The gate validated that examples/polars_workload.py, tests/test_polars.py, and docs/USAGE.md all met the standards, and all checks passed on the first run, so nothing needed to change.

**Unit 8: Monitored PR acceptance and clean merge with no code changes needed**

I used the PR monitor to track my PR #251's status throughout the review phase. On 2026-11-09, the tool reported that the maintainer had approved and merged the PR without requesting any code changes—the solution was accepted as-is after all automated checks (flake8, tests, SonarCloud Quality Gate) passed cleanly. The monitor confirmed that first-time fork contributor approval from the maintainer was the only blocker, which resolved naturally once the maintainer reviewed the work. This taught me that thorough pre-submission validation (via ship-gate) and clear, honest documentation in the PR description meant the code didn't need iteration.

---

## Why I Chose This Issue

I searched for Python repositories working on data profiling and carbon tracking, which matched my interest in both open-source infrastructure and environmental impact. My scout scan on ecotrace/issue#195 returned a reject verdict because the repository has 36 stars, below the rubric's 50-star floor. However, I chose this issue anyway: the repository is clearly active (a commit on 2026-11-02, a release on 2026-11-01), and the issue itself is well-scoped and marked "good first issue" and "help wanted." The work is beginner-friendly—adding a Polars profiling example and docs—but technically interesting: verifying that thread-based 50ms CPU sampling accurately captures multi-threaded Rust execution without GIL interference.

What I hope to learn from this contribution is how to instrument real-world data pipelines for carbon awareness, and to understand the tradeoffs between accuracy and overhead in CPU-utilization sampling. The scout's rubric was sound; the 50-star threshold caught that I was selecting a smaller project, but not that the project was inactive. I would adjust the threshold or add a freshness-date check to the maintainer-active rule to accept actively-maintained smaller projects, rather than silently carrying forward a reject verdict. The tool itself served well for this: it flagged my decision explicitly, forcing me to justify choosing against it.

**Claim:** https://github.com/Zwony/ecotrace/issues/195#issuecomment-6617657583

**Toolkit**

- Tool: issue-scout skill with a five-check rubric (maintainer-active, no-assignee, scope-fits, language, policy). I ran the scan on issues from the ecotrace repository to evaluate which ones were ready to claim.
- What it produced: A detailed scan showing pass/fail grades for each check, with evidence. The scan showed that one issue had an unclear scope-fits check because a related PR had already completed part of the work and deliberately skipped another part, leaving the issue description outdated.
- Difficulties: The scout caught that unclear cases need human judgment—I could not automatically decide if remaining work was still wanted without a maintainer's input. The tool correctly flagged this as a blocker.
- What changed: I added documentation of what would resolve the unclear check (a maintainer comment confirming the scope). This taught me to look for issues where the full scope is decided, not ones where work is still in motion.

---

## Understanding the Issue

**Problem Description**

The issue (#195) asked for a profiling guide and example for Polars, a fast dataframe library that can run "lazy" (you build a query plan first, then run it later with `.collect()`). The repo already had good examples for things like PyTorch, but nothing showed how to use EcoTrace with Polars, and there was no guidance about something tricky: lazy pipelines don't actually run until `.collect()` is called, so if you measure the wrong part of the code you get a measurement of basically nothing.

**Expected Behavior**

A new example file should show someone how to wrap both eager (runs immediately) and lazy (runs on `.collect()`) Polars code with EcoTrace's tracking tools, plus a short doc section explaining how to do it correctly and how trustworthy the numbers actually are.

**Current Behavior**

Before this contribution, there was no Polars example at all, and no documentation calling out the `.collect()` gotcha or how accurate (or not) EcoTrace's CPU sampling is for short bursts of work.

**Affected Components**

- `examples/` — needed a new example file
- `tests/` — needed a new test file to back it up
- `docs/USAGE.md` — needed a new section explaining how to use it

**Toolkit**

- Tool: Claude Code (AI pair-programming assistant, used throughout this contribution with my explicit direction and review at every step)
- What it produced: helped me read the relevant parts of `ecotrace/core.py` quickly, drafted the example/test/docs files for me to review, and helped me investigate the CPU sampling accuracy question with a scratch script
- Difficulties: making sure the AI stayed inside the scope I set (only touch three files) and didn't guess at things it hadn't actually verified in the code
- What changed: I kept a local plan file (`ISSUE_195_PLAN.md`, never committed) with explicit scope guardrails and phase checklists, and reviewed every file before committing it myself

---

## Reproduction Process

**Environment Setup**

Nothing was "broken" to reproduce here — issue #195 was a feature/documentation request, not a bug report. So "reproducing" meant setting up a working copy of the repo and confirming the gap really existed: macOS, Python 3.14.4, a fresh virtual environment with the project's `pyproject.toml` dependencies, plus Polars 2.1.0 installed separately since it's not a required dependency of the project. Setup was simple: clone my fork, `pip install -e .`, then `pip install polars` in the same venv.

No real challenges setting this up — the project's existing `pyproject.toml` and test suite made it easy to get a clean baseline running first.

**Steps to Reproduce**

1. Search `examples/` for any existing Polars example — there isn't one.
2. Search `docs/USAGE.md` for any mention of Polars or lazy evaluation — the only Polars content was in a benchmarks doc, and it didn't explain the `.collect()` timing issue.
3. Observed result: confirmed the gap described in the issue is real and the fix is additive (new files), not a change to existing broken behavior.

**Reproduction Evidence**

- **Commit showing reproduction:** not applicable — this issue had no bug to reproduce, just a documented gap confirmed by searching the repo before writing any code.
- **Screenshots/logs:** none.
- **My findings:** the repo's only related content was `docs/benchmarks/01_pandas_vs_polars.md`, which already used `eco.track_block()` for Polars-style workloads and already mentioned the 50ms sampling caveat. That became my guide for staying consistent with the existing style instead of inventing a new pattern.

**Toolkit**

- Tool: Claude Code
- What it produced: a quick read-through of `examples/`, `docs/`, and the benchmarks doc to confirm the gap and find the existing convention to follow
- Difficulties: none — this step was just reading, not building
- What changed: nothing yet, this just set the direction for everything after

---

## Solution Approach

**Analysis**

I needed to understand how EcoTrace actually measures things before writing an example that uses it correctly. Reading `ecotrace/core.py`, I found:

- `track` (a decorator, around line 975) and `track_block` (a context manager, around line 1434) both use a background thread (`_cpu_monitor_worker`, around line 680) that samples `psutil.Process().cpu_percent()` every `MONITOR_INTERVAL_S` (0.05 seconds, so every 50ms).
- The sampled CPU percentage gets normalized against the total number of logical CPUs on the machine (`_get_avg_cpu_in_range`, around lines 862-863), so "100%" means "all cores busy," not "one core busy."
- Carbon/energy numbers are normally estimated from CPU usage × the CPU's rated TDP (a model), not measured directly — except `@eco.track` can sometimes use real hardware energy counters (Linux RAPL or Apple `powermetrics`) if available, while `track_block()` never does.

The root "cause" here wasn't a bug to fix, it was a documentation and example gap, so my "root cause" analysis was really about understanding this sampling behavior well enough to document it honestly instead of making the Polars example look more precise than it actually is.

**Proposed Solution**

Add one example file, one test file, and one documentation section — nothing else. I deliberately did NOT touch `pyproject.toml`, CI workflow files, `README.md`, changelogs, or `mkdocs.yml`, even though some of those might have been nice-to-haves (like adding Polars as an optional test dependency in CI). Anything outside those three files I wrote down as "out of scope" and left for a separate issue instead of quietly expanding the PR.

**Implementation Plan**

1. Write `examples/polars_workload.py` matching the style of the existing `examples/pytorch_inference.py` (docstring with a "Requires:" line, import guard, constants up top, both decorator and context-manager usage shown).
2. Write `tests/test_polars.py` with `pytest.importorskip("polars")` so the tests skip cleanly in CI (which doesn't install Polars) instead of failing.
3. Add one new section to `docs/USAGE.md` explaining how to use `track_block()`/`@eco.track` with Polars, plus the accuracy caveats.

**Deviations as I built:**

- I originally planned to just pick whichever EcoTrace API "seemed cleaner." Instead I checked `docs/benchmarks/01_pandas_vs_polars.md` first and found it already standardized on `eco.track_block()` for this exact kind of workload, so I matched that instead of introducing a second, inconsistent way of doing the same thing.
- I found a CSV-output side effect I hadn't planned for: `track`/`track_block` always write an audit CSV (`ecotrace_log.csv`) relative to whatever the current working directory happens to be, with no way to redirect it. I added a `tempfile.mkdtemp()` + `os.chdir()` (with a `finally` block to restore the original directory) to the example so running it doesn't leave a stray CSV file in the repo. I used the matching `monkeypatch.chdir(tmp_path)` pattern (already used elsewhere in the test suite) for the tests.
- Partway through, I noticed something worth documenting that wasn't in my original plan: `@eco.track` can use real hardware energy counters when available, but `track_block()` never does — it always falls back to the TDP-based model. Since my example shows both styles side by side, I added a note about this asymmetry to the docs so nobody assumes both methods are equally "exact."

**Toolkit**

- Tool: Claude Code
- What it produced: read the relevant source files for me, drafted each of the three files for review, and helped me stay consistent with the existing `pytorch_inference.py` style
- Difficulties: keeping the AI from suggesting changes outside the three allowed files (it proposed touching `pyproject.toml` once for a "nice to have," which I rejected to keep the PR focused)
- What changed: added the `tempfile`/`os.chdir()` isolation pattern and the hardware-metering asymmetry note, neither of which were in the very first draft plan

---

## Testing Strategy

Since this issue was about adding new capability (an example + docs), not fixing broken behavior, my testing strategy was about proving the new code actually works and doesn't break anything else:

1. **Run the full existing test suite before touching anything**, to get a baseline, then again after each new file, to make sure nothing regressed. Result: no failures at any point, no regressions.
2. **Run the new example script directly**, end-to-end, and visually confirm it prints real output (row counts, carbon totals, and the temp directory it wrote to) instead of crashing or silently doing nothing.
3. **Run the new tests with Polars installed** — all 4 tests passed.
4. **Run the new tests in a clean environment without Polars installed** (mirroring what CI actually does, since CI doesn't install Polars) — all 4 tests skipped cleanly instead of erroring, confirming `pytest.importorskip` worked the way it's supposed to.
5. **Check `git status` and `git diff`** after each step to make sure only the three intended files changed, and that the leftover `ecotrace_log.csv` audit file never leaked into the repo.
6. **Run `flake8`** on both new Python files to catch style issues before committing.
7. A separate, standalone investigation (outside the actual PR) to measure how accurate the CPU utilization numbers really are, described in detail under Implementation Notes below, since "the docs make an honest claim" is itself something that needs evidence, not just a guess.

**Toolkit**

- Tool: Claude Code
- What it produced: ran the test suite and example for me and reported pass/fail counts and `flake8` output
- Difficulties: none significant — the hardest part was making sure I actually looked at the raw output myself instead of trusting a summary
- What changed: nothing in the code — this phase was pure verification

---

## Implementation Notes

**2026-11-05**

Wrote `examples/polars_workload.py`. Built a synthetic dataset (2 million rows, fixed random seed so it's reproducible and needs no internet/downloads) and used it to run an eager group-by, an eager join, and a lazy filter→group_by→sort pipeline. Measured each one with `eco.track_block()`, and also showed the `@eco.track` decorator style on one more function, so both EcoTrace APIs are demonstrated. Made sure `.collect()` runs _inside_ the tracked block, not after it, since that's the whole point of the example. Confirmed with `git status` that no stray `ecotrace_log.csv` ended up in the repo. Committed as `feat(examples): add Polars workload profiling example`.

**2026-11-05**

Wrote `tests/test_polars.py` with 4 tests: that a tracked function returns the same result as the untracked version (both type and values), that a lazy pipeline with `.collect()` inside the tracked scope behaves correctly, and that both `track_block()` and `@eco.track` actually produce a measurement record. Deliberately avoided asserting on exact timing or exact carbon numbers — those vary machine to machine, so I only checked that they're non-negative. Used `pytest.importorskip("polars")` inside each test function (matching an existing pattern already used for another optional dependency in the test suite) so these tests skip instead of fail when Polars isn't installed. Also checked the code works with Python 3.8 syntax, since that's still in the project's CI test matrix (no walrus operator, no newer generic-type syntax). Committed as `test: add Polars profiling integration tests`.

Before writing the docs, I did a separate, honest investigation into how accurate EcoTrace's CPU numbers actually are for Polars workloads, since Polars runs multi-threaded bursts of work that might be too fast for a 50ms sampling interval to catch properly. I wrote a small scratch script (kept outside the repo, never committed) that compared EcoTrace's internal CPU reading against the operating system's own process CPU time, for three kinds of workload: a longer eager pipeline, a longer lazy pipeline, and a short (~55ms) workload. Ran each scenario 5 times. Results: for workloads lasting roughly a second or more, EcoTrace's number landed within about ±25% of the OS ground truth — close enough to be a useful relative signal. For the very short (~55ms) workload, EcoTrace's number was wildly off (about 20x too low), because the sampling thread only caught 0-1 samples in such a short window. This matched what I found reading `_get_avg_cpu_in_range` in the source: with zero samples captured, the code falls back to reporting 100% utilization instead, which is an equally misleading opposite failure. This finding directly shaped the "accuracy" section I added to the docs — I wanted the claim to be honest and backed by evidence, not just "it works great."

**2026-11-05**

Added the `## Profiling Polars Workloads` section to `docs/USAGE.md`, placed right before the existing `## Benchmarks` section to match the doc's flow. Included a minimal working code snippet, a bolded callout about the `.collect()`-must-be-inside-the-tracked-scope rule, a subsection called "How accurate is the CPU figure?" with the real numbers from my scratch investigation (the ±25% finding and the under-150-200ms unreliability finding), and a note that carbon/energy figures are model-based by default and that `track_block()` specifically never uses real hardware energy counters even when they're available (only `@eco.track` can). Linked to the existing `docs/benchmarks/01_pandas_vs_polars.md` for a deeper comparison, using the same relative-link style already used elsewhere in the docs. Committed as `docs: add Polars profiling guide and sampling-accuracy caveats`.

Ran the complete test suite one more time after all three files were in place: 255 passed / 2 skipped with Polars installed (the 2 skips were pre-existing and unrelated), and 251 passed / 6 skipped in a clean environment without Polars (2 pre-existing skips + my 4 new ones skipping as designed). No regressions either way. Also reviewed the full `git diff` line by line to make sure nothing stray — debug prints, local file paths, unrelated edits — had snuck in.

Opened the PR as a **draft** against `main`, ahead of the maintainer's suggested "around Nov 9-10" timing, since the owner had mentioned in a previous (different) PR that they prefer draft PRs opened early so people can see a contributor is actively working on an issue.

After opening the draft, SonarCloud's automated Quality Gate flagged two lines in the example (`rng.randrange(...)` and `rng.choice(...)`) as "security-sensitive pseudorandom number generator usage" (rule `python:S2245`). This is a well-known false positive for this rule — the random numbers here are only used to build fake benchmark data with a fixed seed, nothing security-related. I considered a few options: switching to NumPy's random generator (rejected — adds a new dependency for no real benefit and is scope creep), using Python's `secrets` module (rejected — it isn't seedable, which would break the reproducibility the example depends on), or asking the maintainer to manually mark it "Won't Fix" in SonarCloud (rejected — I don't have the permissions to do that as an outside contributor, and it's not my call to make). I went with the standard, minimal fix: added `# NOSONAR` comments on the two flagged lines plus a short comment explaining why the randomness here isn't a security concern. Re-ran `flake8` and the full example/test suite again to make sure the fix didn't break anything, then pushed it as a separate, clearly-scoped commit: `fix: suppress SonarCloud S2245 false positive in Polars example`. Confirmed afterward that SonarCloud's Quality Gate now shows passing.

**Toolkit**

- Tool: Claude Code
- What it produced: wrote first drafts of all three files and the scratch accuracy-investigation script, which I then reviewed, tested, and committed myself
- Difficulties: catching a subtle CPU-normalization mistake in my own first version of the accuracy-investigation script (I was initially comparing against "100% = one core" instead of "100% = all cores," which made EcoTrace look way less accurate than it actually is) — fixed by rereading `_get_avg_cpu_in_range` in `core.py` carefully and matching its exact normalization
- What changed: fixed the normalization bug in the scratch script before trusting any of its numbers in the docs

---

## Pull Request

**PR Link:** https://github.com/Zwony/ecotrace/pull/251

**PR Description:**

Adds a Polars profiling example and docs guidance for eco.track_block() / @eco.track, including an honest accuracy caveat for the CPU-utilization sampling signal.

**Key Changes:**

- examples/polars_workload.py: synthetic 2M-row dataset with eager and lazy pipeline examples, matching pytorch_inference.py style
- tests/test_polars.py: integration tests with pytest.importorskip("polars") for optional dependency handling
- docs/USAGE.md: new "Profiling Polars Workloads" section with collect()-must-be-inside-scope caveat and accuracy subsection

**Accuracy Finding:** CPU% signal lands within ±25% of ground truth for workloads spanning multiple 50ms sampling intervals; below ~150-200ms, unreliable due to sample collapse.

**Maintainer Feedback:**

- 2026-11-05: SonarCloud's automated Quality Gate flagged a security-rule false positive (`python:S2245`) on two lines of synthetic-data generation in the example.
- 2026-11-05: Fixed with targeted `# NOSONAR` suppressions and an explanatory comment, pushed as a separate commit, re-verified clean.
- 2026-11-09: Maintainer reviewed and merged the PR without requesting any code changes. Clean acceptance after all automated checks passed.

**Status:** Merged (2026-11-09, no code changes requested, clean acceptance)

**Toolkit**

- Tool: Claude Code
- What it produced: helped me draft the PR description and the SonarCloud fix, and explained what the `action_required` status on GitHub Actions meant so I wouldn't worry it was something broken on my end
- Difficulties: figuring out whether "action_required" meant my contribution was broken (it didn't — it's just GitHub gating first-time fork contributors behind maintainer approval)
- What changed: nothing code-related here — this was about understanding GitHub's process correctly before doing anything about it

---

## Learnings and Reflections

**Technical Skills Gained**

- How EcoTrace actually measures CPU usage internally (background-thread sampling, normalized against total logical CPU count) instead of just trusting that "it measures CPU usage" at face value.
- The difference between Polars eager and lazy execution, and why a lazy `.collect()` has to be inside the measured code, not after it.
- The real difference between TDP-modeled energy estimates and hardware-metered energy (RAPL on Linux, `powermetrics` on Apple Silicon), and that this project only supports metered energy through one of its two tracking APIs, not both.
- How to design a small, controlled experiment (not just "run it once and eyeball it") to back up a documentation claim with actual numbers instead of guessing.
- What `# NOSONAR` is for, and when it's the right tool versus when it would be a cop-out.

**Challenges Overcome**

- My first attempt at measuring CPU accuracy gave numbers that looked way worse than reality, because I'd normalized "100% CPU" the wrong way (one core instead of all cores). Fixing that meant actually reading the library's own normalization code line by line instead of assuming I understood it.
- Dealing with a SonarCloud false positive on a PR to a repo I don't maintain, without the ability to just dismiss the warning myself — had to pick the most professional fix available to an outside contributor instead of the easiest one.
- Figuring out GitHub's `action_required` workflow-gating behavior for first-time fork contributors, which looked alarming at first glance but turned out to be completely normal and not something requiring any fix from me.

**What I'd Do Differently Next Time**

I'd write the accuracy-investigation script with the correct CPU normalization from the start by reading the relevant source function fully before writing any comparison code, instead of writing the comparison first and debugging the numbers after. I'd also ask upfront (on the issue, before coding) whether the maintainer had any opinion on which EcoTrace API to use, instead of inferring it from an existing benchmarks doc — in this case it worked out, but it was a bit of a guess.

---

## Resources Used

- [`docs/benchmarks/01_pandas_vs_polars.md`](docs/benchmarks/01_pandas_vs_polars.md) — existing doc in the repo that showed the established convention (`eco.track_block()`, 50ms sampling caveat) I matched instead of inventing a new pattern
- [`examples/pytorch_inference.py`](examples/pytorch_inference.py) — existing example that served as my style template (docstring format, import guard, decorator + context-manager usage shown together)
- [`tests/test_langchain_callback.py`](tests/test_langchain_callback.py) — existing test file that showed the `pytest.importorskip()` pattern for optional dependencies
- PR [#249](https://github.com/Zwony/ecotrace/pull/249) on the same repo — seeing how the maintainer handled a process conflict with another contributor on this same issue helped me understand their preference for early draft PRs and clear communication

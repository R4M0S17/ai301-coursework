# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/123

**Branch**

fix/34-api-rate-limiter-headers

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

19/20 (one run; bar 18/20, PASS). This is the run recorded in `eval-run.txt`.

**Package analysis**

Package pkg-19 (mikefarah/yq#2819, a single-quoted multi-line scalar that loses indentation
at its closing quote). My rubric decided **reject**. The gold label is **accept**, with the
note: "terse but complete: one emitter-state fix mirroring the double-quote writer,
byte-identical round-trip shown with diff, regression scenario added."

The deciding check is "Not tested: captured proof shows the plan's test plan ran on the
branch." The plan's test plan has three steps, and the evidence covers them unevenly:

- Step 1, re-run the issue's input: shown on the branch. The evidence block runs
  `diff <(./yq . source.yml) source.yml && echo IDENTICAL` and prints `IDENTICAL`, which
  matches the expected byte-identical round-trip.
- Step 2, the two controls unchanged: no output is shown. The only text is the claim
  "Controls (double-quoted twin; single-quoted without the trailing blank line) unchanged."
- Step 3, `go test ./...`: no output is shown. The only text is the claim "`go test ./...`
  passes (4108 tests)."

The description does not mark steps 2 or 3 as unrun. So the check's pass condition ("Each
step shows the expected-after value the plan states") is not met, and the verdict is reject.
The gold label reads the decisive repro as enough and treats the controls as a minor claim.
I read the rubric's condition as written, and I did not loosen it to match the label. The
same condition is what rejects pkg-04, pkg-07, pkg-10 and pkg-14, where the plan's main repro
is never shown on the branch.

**Check rationale**

The check, quoted from `skill/rubric.md` as it reads now:

> Not tested: captured proof shows the plan's test plan ran on the branch | Live: `test_evidence.md` read against the test plan in `plan.md`. Eval: the `Test evidence` section read against the `Test plan` in `Plan context`. | The evidence shows real command output from the branch, not main alone, for each step the plan's test plan names. Each step shows the expected-after value the plan states. Placeholder text ("TODO", "paste output here", "tests pass" with no output) counts as no evidence. An unrun step passes only if the description discloses it and the disclosure matches the evidence. | required

Why it reads this way: the condition is per step, not per run, so a test plan with several
steps cannot pass on one shown command. The "tests pass" with no output clause covers the
pattern in pkg-04 and pkg-10, where the evidence is a claim without captured output. The
disclosure clause lets an honestly unrun step pass, as rule 5 of the verdict rule intends.
I don't have a record of the wording before this version, so I can't list the earlier
revisions here. The alternative I weighed was requiring output only for the plan's primary
repro step. That would pass pkg-19, and I did not adopt it, because it would also pass a
branch whose control and suite claims are unshown and undisclosed.

**Trade-offs**

- **A case the check misses:** pkg-19. The gold label accepts it and the rubric rejects it.
  I accept this miss because the description states two results (the controls and the
  `go test` pass count) with no output and no disclosure. Agreement is 19/20, above the 18/20
  bar, and every category has at least one match (not-tested 4/4, clear-accept 6/7,
  silent-drift 4/4, standards-wall 2/2, unreviewable 3/3).
- **Nothing re-run after the change:** I did not change the rubric after the run and did not
  run any `--only` canaries, so the trade-off above is a prediction. Before loosening this
  check, the canary set would be `--only pkg-19,pkg-04,pkg-07,pkg-10,pkg-14`, which checks
  the one miss and the four not-tested packages at about $0.25 each.

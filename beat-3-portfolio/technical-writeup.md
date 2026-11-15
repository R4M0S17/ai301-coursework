# Profiling Polars Workloads in Zwony/ecotrace

## Overview

EcoTrace is an open source Python library that measures the carbon footprint and CPU/energy use of your code. You just wrap a function or a block of code, and it tells you how much CO2 it probably produced. The project already had nice examples for things like PyTorch, but it didn't have one for Polars, which is a popular, fast dataframe library. Polars can also run "lazy," meaning you build a query first and it only actually runs later when you call `.collect()`. That's a bit confusing if you don't know how EcoTrace measures time, so issue [#195](https://github.com/Zwony/ecotrace/issues/195) asked for a proper Polars example and a guide about it.

I picked this issue, built a new example file, a test file, and a new documentation section, and opened [PR #251](https://github.com/Zwony/ecotrace/pull/251). The PR is now merged into `main`. The maintainer reviewed it and merged it with a short, friendly comment saying the implementation and test coverage were excellent.

## What happened

I started by reading the issue carefully and then reading the actual EcoTrace source code before writing anything, because I wanted my example to really show how the tool works instead of just guessing. I found two important things in `ecotrace/core.py`: there is a background thread that samples CPU usage every 50 milliseconds, and the carbon numbers are usually estimated from CPU usage times the CPU's rated power, not measured with real hardware sensors (unless you're on special setups like Linux with root access). I also learned that Polars's `.collect()` function is the thing that actually runs a lazy query — before that, nothing happens. So if you track the wrong part of the code, you measure basically nothing.

I set myself a strict rule before writing any code: only touch three files — the new example, the new test, and one new section in the docs. Nothing else, so the PR stays small and easy to review.

**Test 1:** Here is the core of the lazy pipeline part of my example, from `examples/polars_workload.py`, showing why `.collect()` has to be inside the tracked block, not after it:

```python
def lazy_pipeline(df: pl.DataFrame) -> pl.DataFrame:
    """Build and collect a lazy filter -> group_by -> sort pipeline.

    ``collect()`` is what actually executes a LazyFrame; building the query
    with ``.lazy()`` does no work by itself. It must run inside the tracked
    scope, or the measurement would cover an empty query plan instead of the
    computation it describes.
    """
    query = (
        df.lazy()
        .filter(pl.col("country").is_in(["US", "DE", "GB"]))
        .group_by("category")
        .agg(
            pl.col("amount").sum().alias("total"),
            pl.col("amount").mean().alias("avg"),
        )
        .sort("total", descending=True)
    )
    return query.collect()
```

And in `main()`, I tracked it like this:

```python
with eco.track_block("lazy_pipeline_collect"):
    lazy_result = lazy_pipeline(df)
```

So `.collect()` happens while EcoTrace is watching, not after.

To back up the claims I wanted to put in the docs, I also ran a small accuracy test on the side (not part of the PR itself): I compared EcoTrace's CPU number against the real operating system CPU number for short and long workloads. The result was that for workloads lasting about a second or more, EcoTrace's number was close (within about 25%), but for a very short ~55ms workload, EcoTrace's number was about 20 times too low. That's because the 50ms sampling thread barely catches any samples in such a short window. This is the kind of thing a student learns by testing, not by just reading documentation — the numbers surprised me at first.

I pushed four commits in total, in this order: the example, the test, the docs, and then one more fix.

**Test 2:** After I opened the PR, the project's automated SonarCloud check flagged two lines in my example as "security-sensitive" because I used Python's regular `random` module (`rng.randrange(...)` and `rng.choice(...)`) to build fake test data. This is a known false alarm for that rule, since there's nothing security-related about generating sample numbers for a demo dataset, but the check still failed the Quality Gate. Here is the log/report text I got from the SonarCloud panel on the PR:

```
Quality Gate failed
new_security_rating: A -> C
python:S2245 — "Make sure that using this pseudorandom number generator
is safe here." (2 occurrences, examples/polars_workload.py lines 43-44)
```

**The change I made:** Instead of ignoring it or arguing it was wrong, I treated it like real feedback. I thought about three options: switch to NumPy's random generator (but that adds a new dependency just for this), use Python's `secrets` module (but that can't be seeded, so my example wouldn't be reproducible anymore), or ask the maintainer to dismiss the warning manually (but I don't have permission to do that as an outside contributor). I went with the standard, accepted fix for this exact situation: adding `# NOSONAR` comments on those two lines plus a short comment explaining why the randomness is safe here. I committed that separately as `fix: suppress SonarCloud S2245 false positive in Polars example`, re-ran all my tests and `flake8` again to make sure nothing broke, and pushed it. The Quality Gate turned green after that.

**The result it got:** The maintainer reviewed the PR and merged it into `main`, saying:

> "LGTM! Excellent implementation and test coverage. Thank you for your contribution!"

That felt great, especially since this was my first real contribution to this project, and it meant my example and docs are now part of the real codebase that other people will read and use.

## Limitations and what's next

One honest limitation: my accuracy testing was a quick, local check (5 repeats per scenario on my own machine), not a big formal benchmark. The numbers I put in the docs (±25% for longer workloads, unreliable under ~150-200ms) are a reasonable estimate, not a guarantee for every machine or every Polars version. Someone could build on this by running the same kind of test across different operating systems or CPU types and making the accuracy claim even stronger.

Also, my new tests get skipped automatically in the project's CI pipeline, because CI doesn't install Polars as a dependency. That's fine for now (the tests still run and catch real bugs when Polars is installed locally), but a good next step for the project would be adding Polars as an optional test dependency in CI so these tests actually run on every PR, not just locally. I noted that as a separate, out-of-scope idea rather than doing it myself, since it would have meant touching CI config files, which I had decided to stay away from for this PR.

# Benchmark Environment and Fairness

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Current benchmark environment

The final SW6-vs-FLINT 3.6.0 comparisons were executed in GitHub Actions on:

- runner image: **Ubuntu 24.04**;
- architecture: **x86-64**;
- execution: **single-threaded**;
- SW6 compile flags: `-O3 -march=x86-64-v2 -DNDEBUG`;
- comparator: **FLINT 3.6.0 `arb_exp`**;
- timing repetitions: **5 per input**;
- GMP/MPFR supplied by the Ubuntu runner environment;
- FLINT 3.6.0 built in the Actions environment and reused through the workflow cache.

Each precision/domain comparison timed SW6 and FLINT on the **same hosted runner job**. Absolute times can vary across hosted runners, so the same-job FLINT/SW6 ratio is the primary performance quantity.

## Current benchmark workloads

### Unit domain

- precision: 10K, 20K, 100K decimal digits;
- denominator: `2^32`;
- positive: 100 deterministic random reduced dyadics in `(0,1)`;
- negative: 100 deterministic random reduced dyadics in `(-1,0)`;
- seed: `2026082202`.

### Wide domain

- precision: 10K, 20K, 100K decimal digits;
- denominator: `2^20`;
- positive: 100 deterministic random reduced dyadics in `(1,100)`;
- negative: 100 deterministic random reduced dyadics in `(-100,-1)`;
- seed: `2026082201`.

## Comparator construction

The workflow builds official FLINT 3.6.0 and compiles a small C helper around `arb_exp`. For each case, the exact dyadic input is constructed outside the timed repeated `arb_exp` loop and the median of five calls is reported.

The SW6 side similarly performs reusable cache/context construction outside the timed hot-path measurements.

## Setup transparency

Because SW6 has reusable setup, results are reported in two forms:

1. **steady-state:** hot evaluation time only;
2. **setup-amortized:** one-time shared setup divided by the 200 cases in that domain/precision batch and added to each SW6 call.

The setup-amortized comparison is deliberately included so the speedup is not dependent on hiding cache construction.

## Correctness gate

A case is accepted only when the SW6 result passes both:

- exact-rounded reference agreement; and
- independent higher-precision directed-MPFR certification.

The two final campaigns produced **1,200 / 1,200 certified cases**.

## Endurance environment

The final endurance run was also executed on GitHub-hosted Ubuntu 24.04, using the frozen SW6 candidate, one persistent process and a reusable cache/context. It used 10,000 decimal digits, denominator `2^20`, deterministic seed `2026082203`, and 10,000,000 production calls.

The endurance harness is a different workload from the FLINT benchmark harness. Its absolute throughput is therefore reported as stability evidence only and is not mixed into the FLINT/SW6 speedup tables.

## Interpretation limits

GitHub-hosted runners are virtualized and may vary in physical CPU allocation, frequency behavior and contention. Consequently:

- do not treat small absolute timing differences between separate Actions runs as code regressions without a same-runner comparator;
- use same-job FLINT/SW6 ratios for performance claims;
- keep endurance/stability evidence separate from comparative benchmark evidence.

The current public evidence is single-threaded and Linux/x86-64. Customer hardware and additional platforms should be validated independently before deployment-specific guarantees are made.

# Public Technical Overview

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Purpose

This project evaluates a specialized high-precision kernel for `exp(a)` when the input is represented exactly as a dyadic rational.

The current performance candidate combines the certified n=2 EXP core with a **signed-window-6 (SW6) reusable cache layer**. The cache changes how exact dyadic inputs are represented and reused during evaluation; it does not replace the underlying mathematical construction.

## Current tested operating region

The final same-runner benchmark campaign covers:

- exact dyadic inputs;
- both signs;
- `0 < |a| < 1` and `1 < |a| < 100`;
- 10,000, 20,000 and 100,000 decimal digits;
- single-threaded execution;
- repeated evaluation with a reusable SW6 cache.

The implementation contains wider eligibility guards than the benchmarked `|a| < 100` region, but the public performance claim is intentionally limited to the regions actually benchmarked and certified.

## Performance result

Against FLINT 3.6.0 `arb_exp`, every reported sign/precision aggregate block favored SW6.

Steady-state aggregate FLINT/SW6 speedups ranged from approximately **4.91x to 7.73x** across the final unit- and wide-domain campaigns. When one-time cache construction was conservatively amortized over the 200 calls in each benchmark batch, aggregate speedups remained approximately **3.21x to 4.90x**.

See [`PERFORMANCE_RESULTS.md`](PERFORMANCE_RESULTS.md) for the full table.

## Correctness evidence

The final performance campaigns accepted a timing result only after exact-rounded reference agreement and an independent directed-MPFR check.

Across the current benchmark set:

**1,200 / 1,200 cases passed both correctness gates.**

## Endurance evidence

At 10,000 decimal digits, the SW6 candidate completed a separate **10,000,000-call persistent-process endurance test** over a mixed workload covering positive/negative unit-domain and wide-domain inputs.

The run completed:

- 10,000,000 / 10,000,000 successful calls;
- 0 invalid outputs;
- 10,000 / 10,000 sampled directed-MPFR checks passed;
- 0 correctness failures;
- no progressive RSS growth after the 1M-call checkpoint.

This endurance result is a stability qualification, not a benchmark against FLINT.

## Intended use

The natural deployment model is a **narrowly dispatched acceleration path** inside a larger numerical system:

- use SW6 where the input form, magnitude and precision fall inside the validated fast region;
- retain an incumbent general-purpose exponential implementation as the fallback elsewhere.

That avoids requiring a downstream user to replace its entire exponential stack in order to exploit the specialized fast path.

## Reference implementation

The current primary benchmark comparator is **FLINT 3.6.0 `arb_exp`**.

Each benchmark precision was run on a GitHub-hosted Ubuntu 24.04 job, with SW6 and FLINT timed on the same runner and identical exact inputs supplied to both implementations.

## Maturity

The SW6 benchmark and 10M endurance qualification are complete for the documented test regions. Cross-platform reproduction, customer-specific workloads, multi-thread behavior and production integration remain separate qualification dimensions.

## Confidentiality boundary

This public description intentionally discusses capability, validation, release boundaries and measured behavior only.

It does not disclose the proprietary mathematical derivation or source-level implementation details.

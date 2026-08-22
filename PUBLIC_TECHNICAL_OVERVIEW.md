# Public Technical Overview

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Purpose

This project evaluates a specialized high-precision kernel for `exp(a)` when the input is represented exactly as a dyadic rational.

The current performance candidate is an **optimized exact-dyadic EXP kernel**. This public overview intentionally does not describe the mathematical derivation, internal representation, or source-level optimization architecture.

## Current tested operating region

The final same-runner benchmark campaign covers:

- exact dyadic inputs;
- both signs;
- `0 < |a| < 1` and `1 < |a| < 100`;
- 10,000, 20,000 and 100,000 decimal digits;
- single-threaded execution;
- repeated evaluation with reusable initialized state.

The implementation contains wider eligibility guards than the benchmarked `|a| < 100` region, but the public performance claim is intentionally limited to the regions actually benchmarked and certified.

## Performance result

Against FLINT 3.6.0 `arb_exp`, every reported sign/precision aggregate block favored the optimized kernel.

Steady-state aggregate FLINT/kernel speedups ranged from approximately **4.91x to 7.73x** across the final unit- and wide-domain campaigns. When one-time reusable setup was conservatively amortized over the 200 calls in each benchmark batch, aggregate speedups remained approximately **3.21x to 4.90x**.

See [`PERFORMANCE_RESULTS.md`](PERFORMANCE_RESULTS.md) for the full table.

## Correctness evidence

The final performance campaigns accepted a timing result only after exact-rounded reference agreement and an independent directed-MPFR check.

Across the current benchmark set:

**1,200 / 1,200 cases passed both correctness gates.**

## Endurance evidence

At 10,000 decimal digits, the optimized candidate completed a separate **10,000,000-call persistent-process endurance test** over a mixed workload covering positive/negative unit-domain and wide-domain inputs.

The run completed:

- 10,000,000 / 10,000,000 successful calls;
- 0 invalid outputs;
- 10,000 / 10,000 sampled directed-MPFR checks passed;
- 0 correctness failures;
- no progressive RSS growth after the 1M-call checkpoint.

This endurance result is a stability qualification, not a benchmark against FLINT.

## Implementation eligibility

For exact dyadic `a=A/2^k`, the current implementation guard requires denominator exponent `0 <= k <= 64`, numerator magnitude within 64 bits, and `bit_length(|A|)-k <= 20`. This implies **`|a| < 2^20` (approximately 1,048,576)** subject to the other guards.

That is an implementation ceiling, not a validated performance envelope. Current comparative evidence extends only through `|a| < 100`.

## Intended use

The natural deployment model is a **narrowly dispatched acceleration path** inside a larger numerical system:

- use the optimized kernel where the input form, magnitude and precision fall inside the validated fast region;
- retain an incumbent general-purpose exponential implementation as the fallback elsewhere.

## Reference implementation

The current primary benchmark comparator is **FLINT 3.6.0 `arb_exp`**.

Each benchmark precision was run on a GitHub-hosted Ubuntu 24.04 job, with the optimized kernel and FLINT timed on the same runner and identical exact inputs supplied to both implementations.

## Maturity

The benchmark and 10M endurance qualification are complete for the documented test regions. Cross-platform reproduction, customer-specific workloads, multi-thread behavior and production integration remain separate qualification dimensions.

## Confidentiality boundary

This public description intentionally discusses capability, validation, release boundaries and measured behavior only.

It does not disclose the proprietary mathematical derivation, internal representation, or source-level implementation details.

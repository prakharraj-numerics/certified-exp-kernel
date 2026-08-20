# Certified High-Precision Exponential Kernel

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

Experimental high-precision software for computing `exp(a)` when the input is an exact rational number.

The current implementation is designed as a specialized fast path for exact-rational inputs at high precision. It returns a certified numerical interval containing the true result and has been benchmarked against FLINT 3.6.0 `arb_exp`.

## Current evidence

The validation campaign covers:

- requested precision from **1,000 to 500,000 decimal digits**;
- positive and negative exact-rational inputs;
- integer magnitudes tested up to **10,000**;
- large noninteger magnitudes tested into the several-thousand range;
- more than **1,000 correctness-checked evaluations** across the full research campaign;
- independent higher-precision containment checks.

A combined direct/extended dispatcher validation over the original 400-case random set plus a 12-case large-integer range produced:

- **411 / 412 wins** against the reference implementation;
- **0 correctness failures**.

The one residual loss belongs to a narrow input subclass described in
[`SUPPORTED_SCOPE.md`](SUPPORTED_SCOPE.md)

## Precision scaling

On a fixed six-input representative set:

| Decimal digits | Geometric-mean time ratio | Median time ratio | Wins |
|---:|---:|---:|---:|
| 1,000 | 0.82 | 0.89 | 4 / 6 |
| 2,000 | 0.75 | 0.77 | 5 / 6 |
| 5,000 | 0.61 | 0.64 | 6 / 6 |
| 10,000 | 0.47 | 0.49 | 6 / 6 |
| 20,000 | 0.38 | 0.41 | 6 / 6 |
| 50,000 | 0.36 | 0.37 | 6 / 6 |
| 100,000 | 0.36 | 0.33 | 6 / 6 |
| 200,000 | 0.33 | 0.29 | 6 / 6 |

`ratio = prototype time / FLINT time`; values below 1.0 mean the prototype was faster.

The advantage was not universal at very low precision. From approximately 5,000 digits upward, every value in this representative precision-scaling set won.

## Certification

Every reported benchmark result was required to satisfy the requested accuracy and to agree with the reference interval.

A separate independent verification recomputed selected cases from scratch at higher precision and checked containment. The documented direct-kernel campaign reports **40 / 40 independent containment checks passed**.

## Current deployment profile

The prototype is currently most suitable for:

- exact-rational inputs;
- numerator and denominator well within the implementation's current 64-bit capacity;
- precision of at least a few thousand decimal digits;
- single-threaded high-precision numerical workloads.

It is **not** presented as a universal replacement for a general-purpose exponential implementation.

## Proprietary details

The public repository describes capability, validation results, tested operating
ranges, and limitations. Proprietary mathematical and implementation details are
intentionally omitted.

## Documentation

- [`Public technical overview`](PUBLIC_TECHNICAL_OVERVIEW.md)
- [`Performance results`](PERFORMANCE_RESULTS.md)
- [`Validation summary`](VALIDATION_SUMMARY.md)
- [`Supported scope and limitations`](SUPPORTED_SCOPE.md)
- [`Benchmark environment and fairness`](ENVIRONMENT_AND_FAIRNESS.md)
## Status

**Experimental prototype — validated on the documented test environment and input envelope.**

Cross-machine portability and broader adversarial testing remain open work before any claim of general deployment readiness.

## Version

Draft public package: **0.1.0-draft**

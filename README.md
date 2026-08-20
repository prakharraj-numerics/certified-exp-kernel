# Certified High-Precision Exponential Kernel

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

Experimental high-precision software for computing `exp(a)` when the input is an exact rational number.

The project evaluates a specialized fast path for exact-rational inputs at high precision. The kernel returns a certified numerical interval containing the true result and has been benchmarked against FLINT 3.6.0 `arb_exp`.

## Current canonical release line

The **current canonical executable is the rigorous direct kernel only**.

It does **not** yet include the later large-magnitude reduction/dispatch layer explored and validated in the broader research campaign. Large input magnitude is therefore a known performance weakness of the current direct release; this is a performance limitation, not a known correctness failure.

A current-build smoke test at 5,000 decimal digits deliberately included `a = 100`. The result passed higher-precision Arb containment, while the direct kernel was slower than FLINT on that case (`ours / FLINT = 1.1874`).

Separately, the present exact-rational interface is limited to machine-word-sized numerator/denominator components (approximately 64-bit), and performance degrades as rational height approaches that ceiling.

See [`SUPPORTED_SCOPE.md`](SUPPORTED_SCOPE.md) for the distinction between **large argument magnitude** and **large rational height**.

## Research-campaign evidence

The broader validation campaign covers:

- requested precision from **1,000 to 500,000 decimal digits**;
- positive and negative exact-rational inputs;
- more than **1,000 correctness-checked evaluations** across the full research campaign;
- independent higher-precision containment checks;
- direct-kernel testing and, separately, a later experimental large-magnitude extension.

The historical combined direct/extended dispatcher campaign over the original 400-case random set plus a 12-case large-integer range produced:

- **411 / 412 wins** against the reference implementation;
- **0 correctness failures**.

Those **411 / 412** results belong to the broader historical research campaign. They are **not measurements of the current direct-only canonical executable**.

The one residual historical loss belongs to a narrow input subclass described in [`SUPPORTED_SCOPE.md`](SUPPORTED_SCOPE.md).

## Precision scaling

On a fixed six-input representative set from the documented research campaign:

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

A separate higher-precision verification of the documented direct-kernel campaign reports **40 / 40 containment checks passed**.

The current canonical direct executable also includes a higher-precision Arb containment check in its evaluation driver.

## Current deployment profile

The current direct release is most suitable for:

- exact-rational inputs;
- numerator and denominator well within the implementation's current machine-word capacity;
- moderate input magnitude, pending integration of the later magnitude-reduction layer;
- precision of at least a few thousand decimal digits;
- single-threaded high-precision numerical workloads.

It is **not** presented as a universal replacement for a general-purpose exponential implementation.

## Proprietary details

The public repository describes capability, validation results, tested operating ranges, release boundaries, and limitations. Proprietary mathematical and implementation details are intentionally omitted.

## Documentation

- [`Public technical overview`](PUBLIC_TECHNICAL_OVERVIEW.md)
- [`Performance results`](PERFORMANCE_RESULTS.md)
- [`Validation summary`](VALIDATION_SUMMARY.md)
- [`Supported scope and limitations`](SUPPORTED_SCOPE.md)
- [`Benchmark environment and fairness`](ENVIRONMENT_AND_FAIRNESS.md)

## Status

**Experimental prototype — current canonical line is the rigorous direct kernel; large-magnitude extension remains separate future integration work.**

Cross-machine portability, broader adversarial testing, integration of the validated large-magnitude strategy into the current canonical source line, and a wider-than-machine-word rational front end remain open work before any claim of general deployment readiness.

## Version

Draft public package: **0.1.1-draft-direct-synchronized**

# Certified High-Precision Exponential Kernel

> **DRAFT - FOR REVIEW BEFORE PUBLIC RELEASE**

Experimental high-precision software for evaluating `exp(x)` on exact rational inputs, with independently checked correctness evidence and same-runner performance comparisons against FLINT 3.6.0.

## Current benchmark headline

The current headline benchmark uses **exact reduced non-dyadic rational inputs**. The public repository intentionally discloses no internal mathematical or implementation method.

`FLINT / kernel > 1` means the kernel was faster.

| Domain | Digits | Combined | Positive | Negative |
|---|---:|---:|---:|---:|
| `0 < |x| < 1` | 10,000 | **1.5319x** | **1.5431x** | **1.5210x** |
| `0 < |x| < 1` | 20,000 | **1.9233x** | **1.9351x** | **1.9119x** |
| `1 < |x| < 100` | 10,000 | **1.4388x** | **1.4361x** | **1.4415x** |
| `1 < |x| < 100` | 20,000 | **1.8399x** | **1.8471x** | **1.8326x** |

Across these four 100-case blocks:

- **400 / 400 wins vs FLINT 3.6.0**;
- **400 / 400 overlap checks passed**;
- **400 / 400 independent higher-precision containment checks passed**;
- both positive and negative inputs were exercised in every block.

Each block used 100 deterministic random exact reduced non-dyadic rationals: **50 positive + 50 negative**, with each sign split between odd and even non-power-of-two denominators. Five timing repetitions per input and one thread were used. Kernel and FLINT timings were taken in the same GitHub Actions job.

See `PERFORMANCE_RESULTS.md` and `ENVIRONMENT_AND_FAIRNESS.md` for the complete benchmark interpretation.

## Separate endurance qualification

A previously frozen exact-input evaluator completed a separate **10,000,000-call persistent-process endurance run at 10,000 decimal digits** over a mixed four-region workload.

Result:

- **10,000,000 / 10,000,000 successful calls**;
- **0 invalid outputs**;
- **256 / 256 preflight correctness checks passed**;
- **10,000 / 10,000 sampled directed-MPFR checks passed**;
- **0 correctness failures**;
- production time: **1,231.122 s**;
- throughput: **8,122.7 calls/s**;
- RSS: **5,960 KB -> 6,168 KB**, unchanged from the 1M checkpoint through 10M.

That endurance workload used exact dyadic inputs. It is reported as stability evidence and is **not** presented as generic-rational endurance evidence or as a FLINT speed comparison.

## Current tested scope

The current public comparative evidence covers:

- exact rational inputs `p/q`, including genuine non-dyadic rationals;
- both signs;
- `0 < |x| < 1` and `1 < |x| < 100`;
- 10,000 and 20,000 decimal digits for the current non-dyadic qualification;
- single-threaded execution;
- deterministic reproducible input generation;
- same-job comparison against FLINT 3.6.0 `arb_exp`;
- independent higher-precision containment checking.

The current benchmark generator used positive denominators below `2^31` and machine-word signed numerators. This is the currently published qualification envelope, not a statement that larger exact-rational heights are impossible.

## Performance setup accounting

The kernel has a one-time benchmark setup cost at a fixed precision. The headline ratios above compare repeated per-input evaluation after that setup. To keep the setup visible, `PERFORMANCE_RESULTS.md` also reports the ratio obtained when one complete setup is charged across the 100-call block.

## Intended deployment model

The kernel is intended for use as a high-precision exponential component inside a larger numerical system. Deployment-specific integration, customer hardware, additional input-height ranges and multi-thread behavior remain separate qualification activities.

## Disclosure boundary

This public repository contains capability, benchmark methodology, validation results, operating scope and limitations only. **No internal method is disclosed.**

## Documentation

- `PUBLIC_TECHNICAL_OVERVIEW.md`
- `PERFORMANCE_RESULTS.md`
- `VALIDATION_SUMMARY.md`
- `SUPPORTED_SCOPE.md`
- `ENVIRONMENT_AND_FAIRNESS.md`

## Status

**Mixed-sign non-dyadic qualification is complete for the documented 10K/20K test regions.** The separate 10M endurance record remains valid for its stated exact-dyadic workload.

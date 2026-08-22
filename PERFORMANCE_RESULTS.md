# Performance Results

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

This file records the current benchmark set for the **optimized exact-dyadic EXP candidate**.

All speedups are reported as:

`FLINT 3.6.0 time / optimized-kernel time`

A value greater than 1.0 means the optimized kernel was faster.

## Benchmark design

Two deterministic exact-dyadic domains were tested at 10,000, 20,000 and 100,000 decimal digits:

1. **unit domain:** 100 positive inputs in `(0,1)` and 100 negative inputs in `(-1,0)` at each precision, denominator `2^32`, seed `2026082202`;
2. **wide domain:** 100 positive inputs in `(1,100)` and 100 negative inputs in `(-100,-1)` at each precision, denominator `2^20`, seed `2026082201`.

Each input was timed with five repetitions. The optimized kernel and FLINT 3.6.0 were run on the same GitHub Actions job for each precision. The FLINT comparator was the public C-level `arb_exp` routine.

Correctness was a hard gate: every benchmark output had to pass exact-rounded agreement and an independent directed-MPFR certification check.

## Unit domain: `0 < |a| < 1`

### Steady-state aggregate speedup

| Digits | Positive | Negative |
|---:|---:|---:|
| 10,000 | **5.0869x** | **5.0894x** |
| 20,000 | **5.5481x** | **5.4853x** |
| 100,000 | **4.9064x** | **4.9601x** |

### Aggregate speedup including 1/200 of shared setup per call

| Digits | Positive | Negative |
|---:|---:|---:|
| 10,000 | **3.3037x** | **3.2611x** |
| 20,000 | **3.5690x** | **3.5230x** |
| 100,000 | **3.2145x** | **3.2396x** |

The unit-domain campaign recorded **600 / 600 steady-state wins** and **600 / 600 exact-rounded + directed-MPFR certified cases**.

## Wide domain: `1 < |a| < 100`

### Steady-state aggregate speedup

| Digits | Positive | Negative |
|---:|---:|---:|
| 10,000 | **7.1336x** | **7.4786x** |
| 20,000 | **7.7109x** | **7.7258x** |
| 100,000 | **7.2250x** | **7.2949x** |

### Aggregate speedup including 1/200 of shared setup per call

| Digits | Positive | Negative |
|---:|---:|---:|
| 10,000 | **4.0689x** | **4.1357x** |
| 20,000 | **4.8963x** | **4.8765x** |
| 100,000 | **4.6113x** | **4.6304x** |

The wide-domain campaign produced **600 / 600 exact-rounded + directed-MPFR certified cases**. Every sign/precision aggregate block favored the optimized kernel.

## Combined current benchmark evidence

Across the two final campaigns:

- benchmark cases: **1,200**;
- certified cases: **1,200 / 1,200**;
- tested precision: **10K, 20K, 100K decimal digits**;
- signs: **positive and negative**;
- tested magnitude regions: **`0 < |a| < 1` and `1 < |a| < 100`**;
- threads: **1**.

The strongest aggregate steady-state block in this set was approximately **7.73x** FLINT/kernel at 20,000 digits in the negative wide-domain sample. The smallest current steady-state aggregate block was approximately **4.91x**, still in favor of the optimized kernel.

## Setup interpretation

The implementation performs reusable one-time setup before repeated evaluation. The steady-state figures exclude that setup from individual hot-path timings. To make the setup cost visible, the benchmark also reports an intentionally conservative amortization of **1/200 of total shared setup cost per call**, corresponding to the 200-case batch.

The setup-amortized results remain favorable in every reported sign/precision block.

## Endurance throughput is not a benchmark ratio

The separate 10M endurance run at 10,000 digits produced **8,122.7 calls/s** over a mixed four-region workload. That figure belongs to a different qualification harness and must not be compared directly with the FLINT benchmark ratios above.

## Confidentiality boundary

This document intentionally reports observable benchmark behavior only. It does not describe the internal mathematical construction, data representation, recoding strategy, cached objects, or source-level optimization architecture.

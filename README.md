# Certified High-Precision Exponential Kernel

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

Experimental high-precision software for evaluating `exp(a)` on exact dyadic inputs, with independent correctness certification and a specialized optimized path for repeated high-precision work.

## Current performance line

The current benchmarked candidate is the **optimized exact-dyadic EXP kernel**. Proprietary mathematical and implementation details are intentionally not disclosed in this public repository.

The current headline evidence is from same-runner comparisons against **FLINT 3.6.0 `arb_exp`** on GitHub-hosted Ubuntu 24.04, one thread, five timing repetitions per input, with identical exact inputs and requested precision supplied to both implementations.

### Final optimized-kernel vs FLINT 3.6.0 benchmark set

`FLINT / kernel` greater than 1 means the optimized kernel was faster.

| Domain | Digits | Positive steady-state | Negative steady-state | Positive incl. amortized setup | Negative incl. amortized setup |
|---|---:|---:|---:|---:|---:|
| `0 < |a| < 1` | 10,000 | **5.0869x** | **5.0894x** | **3.3037x** | **3.2611x** |
| `0 < |a| < 1` | 20,000 | **5.5481x** | **5.4853x** | **3.5690x** | **3.5230x** |
| `0 < |a| < 1` | 100,000 | **4.9064x** | **4.9601x** | **3.2145x** | **3.2396x** |
| `1 < |a| < 100` | 10,000 | **7.1336x** | **7.4786x** | **4.0689x** | **4.1357x** |
| `1 < |a| < 100` | 20,000 | **7.7109x** | **7.7258x** | **4.8963x** | **4.8765x** |
| `1 < |a| < 100` | 100,000 | **7.2250x** | **7.2949x** | **4.6113x** | **4.6304x** |

Each domain campaign used 100 positive and 100 negative deterministic random exact dyadics at each of the three precisions. Across the two campaigns, **1,200 / 1,200 benchmark cases passed both exact-rounded overlap and independent directed-MPFR certification**. The unit-domain campaign recorded **600 / 600 steady-state wins** against FLINT.

See [`PERFORMANCE_RESULTS.md`](PERFORMANCE_RESULTS.md) and [`ENVIRONMENT_AND_FAIRNESS.md`](ENVIRONMENT_AND_FAIRNESS.md) for the exact interpretation of steady-state and setup-amortized ratios.

## Final endurance qualification

The frozen optimized candidate completed a **10,000,000-call persistent-process endurance run at 10,000 decimal digits** over a mixed workload spanning all four tested regions:

- `+(0,1)`
- `-(0,1)`
- `+(1,100)`
- `-(1,100)`

Result:

- **10,000,000 / 10,000,000 successful calls**
- **0 invalid outputs**
- **256 / 256 preflight correctness checks passed**
- **10,000 / 10,000 sampled directed-MPFR checks passed**
- **0 correctness failures**
- production time: **1,231.122 s**
- throughput: **8,122.7 calls/s**
- one-time reusable setup: **16.19 ms**
- RSS: **5,960 KB -> 6,168 KB**, with RSS unchanged from the 1M checkpoint through 10M

This is an endurance/stability qualification, not a speed comparison against FLINT. Benchmark and endurance evidence are intentionally kept separate.

## Current tested scope

The current published evidence covers:

- exact dyadic inputs;
- both positive and negative arguments;
- benchmark regions `0 < |a| < 1` and `1 < |a| < 100`;
- 10,000, 20,000 and 100,000 decimal-digit benchmark precision;
- single-threaded repeated evaluation with reusable initialized state;
- independent directed-MPFR correctness verification.

The current implementation accepts a wider exact-dyadic envelope than the benchmarked `|a| < 100` region. The hard implementation eligibility conditions include denominator exponent `0 <= k <= 64`, numerator magnitude within 64 bits, and `bit_length(|A|)-k <= 20`, implying **`|a| < 2^20` (about 1,048,576)** subject to the other guards. This is an implementation limit, not a performance claim beyond the tested region.

## Intended deployment model

This kernel is best treated as a **specialized acceleration path** inside a broader numerical system. A general-purpose implementation can remain as the fallback outside the documented dispatch region.

It is not presented as a universal replacement for every exponential workload or every precision regime.

## Proprietary details

The public repository describes capability, benchmark methodology, validation, tested operating ranges and limitations. Proprietary mathematical derivation, algorithmic architecture and implementation details are intentionally omitted.

## Documentation

- [`Public technical overview`](PUBLIC_TECHNICAL_OVERVIEW.md)
- [`Performance results`](PERFORMANCE_RESULTS.md)
- [`Validation summary`](VALIDATION_SUMMARY.md)
- [`Supported scope and limitations`](SUPPORTED_SCOPE.md)
- [`Benchmark environment and fairness`](ENVIRONMENT_AND_FAIRNESS.md)

## Status

**Benchmark and 10M endurance qualification are complete for the documented exact-dyadic test regions.** External/customer-specific validation, additional platforms, and integration hardening remain separate deployment activities.

## Version

Draft public package: **0.2.0-draft-qualified**

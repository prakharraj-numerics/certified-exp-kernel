# Certified High-Precision Exponential Kernel

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

Experimental high-precision software for evaluating `exp(a)` on exact dyadic inputs, with independent correctness certification and a specialized cached fast path for repeated high-precision work.

## Current performance line: SW6

The current benchmarked candidate is the **signed-window-6 (SW6) cached EXP kernel**. The underlying mathematical spine remains the certified n=2 EXP engine; SW6 changes representation and reusable evaluation structure rather than replacing the mathematical construction.

The current headline evidence is from same-runner comparisons against **FLINT 3.6.0 `arb_exp`** on GitHub-hosted Ubuntu 24.04, one thread, five timing repetitions per input, with identical exact inputs and requested precision supplied to both implementations.

### Final SW6 vs FLINT 3.6.0 benchmark set

`FLINT / SW6` greater than 1 means SW6 was faster.

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

The frozen SW6 candidate also completed a **10,000,000-call persistent-process endurance run at 10,000 decimal digits** over a mixed workload spanning all four tested regions:

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
- SW6 cache setup: **16.19 ms**
- RSS: **5,960 KB -> 6,168 KB**, with RSS unchanged from the 1M checkpoint through 10M

This is an endurance/stability qualification, not a speed comparison against FLINT. Benchmark and endurance evidence are intentionally kept separate.

## Current tested scope

The current published evidence covers:

- exact dyadic inputs;
- both positive and negative arguments;
- benchmark regions `0 < |a| < 1` and `1 < |a| < 100`;
- 10,000, 20,000 and 100,000 decimal-digit benchmark precision;
- single-threaded repeated evaluation with reusable SW6 cache state;
- independent directed-MPFR correctness verification.

The implementation contains eligibility guards wider than the benchmarked `|a| < 100` region, but those wider bounds are not presented here as equally validated performance claims.

## Intended deployment model

This kernel is best treated as a **specialized acceleration path** inside a broader numerical system. A general-purpose implementation can remain as the fallback outside the documented dispatch region.

It is not presented as a universal replacement for every exponential workload or every precision regime.

## Historical evidence

Earlier direct-kernel, recovered-dyadic and large-magnitude experiments remain part of the research provenance, but they are **not the current headline benchmark set**. Current performance claims in this repository refer to the final SW6 campaign above unless explicitly labeled historical.

## Proprietary details

The public repository describes capability, benchmark methodology, validation, tested operating ranges and limitations. Proprietary mathematical derivation and implementation details are intentionally omitted.

## Documentation

- [`Public technical overview`](PUBLIC_TECHNICAL_OVERVIEW.md)
- [`Performance results`](PERFORMANCE_RESULTS.md)
- [`Validation summary`](VALIDATION_SUMMARY.md)
- [`Supported scope and limitations`](SUPPORTED_SCOPE.md)
- [`Benchmark environment and fairness`](ENVIRONMENT_AND_FAIRNESS.md)

## Status

**SW6 benchmark and 10M endurance qualification complete for the documented exact-dyadic test regions.** External/customer-specific validation, additional platforms, and integration hardening remain separate deployment activities.

## Version

Draft public package: **0.2.0-draft-sw6-qualified**

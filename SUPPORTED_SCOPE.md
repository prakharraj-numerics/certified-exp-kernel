# Supported Scope and Limitations

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Current input form

The current optimized performance line operates on **exact dyadic inputs** of the form

`a = A / 2^k`.

Both positive and negative arguments are supported in the tested campaigns.

## Current benchmarked scope

The final same-runner optimized-kernel-vs-FLINT 3.6.0 evidence covers:

- `0 < |a| < 1`;
- `1 < |a| < 100`;
- 10,000, 20,000 and 100,000 decimal digits;
- 100 positive + 100 negative deterministic random inputs per domain and precision;
- single-threaded execution;
- repeated evaluation with reusable initialized state.

Across those two campaigns, **1,200 / 1,200 benchmark cases passed exact-rounded agreement and directed-MPFR certification**.

## Endurance-tested scope

The final 10M endurance qualification used:

- 10,000 decimal digits;
- exact dyadics with denominator `2^20`;
- a deterministic mixed workload spanning `+(0,1)`, `-(0,1)`, `+(1,100)` and `-(1,100)`;
- one persistent process and reusable initialized state.

It completed **10,000,000 / 10,000,000 calls** with zero invalid outputs and zero sampled correctness failures.

## Implementation eligibility versus validated performance scope

For an exact dyadic `A/2^k`, the current hard eligibility conditions are:

- denominator exponent `0 <= k <= 64`;
- numerator magnitude fitting within **64 bits**;
- `bit_length(|A|) - k <= 20`.

The last condition implies a current implementation magnitude ceiling of

**`|a| < 2^20` (approximately 1,048,576)**,

subject to the other numerator/denominator guards above. The same magnitude rule applies to positive and negative inputs.

This **is an implementation ceiling, not a validated performance envelope**. Current comparative benchmark and endurance evidence extends only through `|a| < 100`. Inputs above 100 but below the implementation ceiling are not rejected merely because they exceed the benchmark range, but they have not yet received the same published performance qualification.

## Precision

The current headline benchmark set is at **10K, 20K and 100K decimal digits**.

## Rational form

The current fast path is specifically an **exact-dyadic** path. Arbitrary non-power-of-two rational denominators are outside the current headline release claim.

## Setup and repeated-call behavior

The optimized evaluator performs reusable setup before repeated evaluation. Current performance reporting therefore gives both:

- steady-state evaluation speedup; and
- a conservative setup-amortized speedup that assigns 1/200 of shared setup cost to each call in the benchmark batch.

Both versions of the aggregate comparison favored the optimized kernel in every reported sign/precision block.

## Current non-goals

This prototype should not be represented as:

- a universal replacement for `arb_exp`;
- a claim about arbitrary rational denominators;
- a performance guarantee for every magnitude permitted by internal eligibility checks;
- a multi-thread scaling result;
- a cross-platform performance guarantee;
- a claim that 10M successful calls prove failures are impossible.

The intended commercial use is a specialized acceleration path with a general-purpose fallback outside the validated dispatch region.

## Confidentiality boundary

The public scope deliberately omits the internal mathematical construction, representation and optimization architecture.

# Supported Scope and Limitations

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Current input form

The current SW6 performance line operates on **exact dyadic inputs** of the form

`a = A / 2^k`.

Both positive and negative arguments are supported in the tested campaigns.

## Current benchmarked scope

The final same-runner SW6-vs-FLINT 3.6.0 evidence covers:

- `0 < |a| < 1`;
- `1 < |a| < 100`;
- 10,000, 20,000 and 100,000 decimal digits;
- 100 positive + 100 negative deterministic random inputs per domain and precision;
- single-threaded execution;
- reusable signed-window cache state.

Across those two campaigns, **1,200 / 1,200 benchmark cases passed exact-rounded agreement and directed-MPFR certification**.

## Endurance-tested scope

The final 10M endurance qualification used:

- 10,000 decimal digits;
- exact dyadics with denominator `2^20`;
- a deterministic mixed workload spanning `+(0,1)`, `-(0,1)`, `+(1,100)` and `-(1,100)`;
- one persistent process and one reusable SW6 cache/context.

It completed **10,000,000 / 10,000,000 calls** with zero invalid outputs and zero sampled correctness failures.

## Implementation eligibility versus validated performance scope

The current SW6 cache implementation has explicit hard eligibility guards. For an exact dyadic `A/2^k`, the current guard requires:

- denominator exponent `0 <= k <= 64`;
- numerator magnitude fitting within **64 bits**;
- `bit_length(|A|) - k <= 20`.

The last condition implies a current implementation magnitude ceiling of

**`|a| < 2^20` (approximately 1,048,576)**,

subject to the other numerator/denominator guards above. The same magnitude rule applies to positive and negative inputs.

This **is an implementation ceiling, not a validated performance envelope**. Current comparative benchmark and endurance evidence extends only through `|a| < 100`. Inputs above 100 but below the implementation ceiling are therefore not rejected merely because they exceed the benchmark range, but they have not yet received the same published performance qualification.

Accordingly, the public performance claim remains limited to the actual tested regions above.

## Precision

The current SW6 headline benchmark set is at **10K, 20K and 100K decimal digits**.

Earlier research explored a broader precision range, but those older measurements belong to different source revisions and are retained only as historical evidence.

## Rational form

The current SW6 fast path is specifically an **exact-dyadic** path. Arbitrary non-power-of-two rational denominators are outside the current headline release claim.

## Setup and repeated-call behavior

SW6 builds reusable cached values before the hot evaluation loop. Current performance reporting therefore gives both:

- steady-state evaluation speedup; and
- a conservative setup-amortized speedup that assigns 1/200 of the shared cache setup cost to each call in the benchmark batch.

Both versions of the aggregate comparison favored SW6 in every reported sign/precision block.

## Current non-goals

This prototype should not be represented as:

- a universal replacement for `arb_exp`;
- a claim about arbitrary rational denominators;
- a performance guarantee for every magnitude permitted by internal eligibility checks;
- a multi-thread scaling result;
- a cross-platform performance guarantee;
- a claim that 10M successful calls prove failures are impossible.

The intended commercial use is a specialized acceleration path with a general-purpose fallback outside the validated dispatch region.

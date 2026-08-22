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

The implementation contains internal eligibility guards wider than the published benchmark region, including bounded denominator exponent, numerator bit length and magnitude. Those guards define where the cache implementation can execute; they do **not** mean that every point inside the wider implementation envelope has received the same benchmark and endurance coverage.

Accordingly, the public performance claim is limited to the actual tested regions above.

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

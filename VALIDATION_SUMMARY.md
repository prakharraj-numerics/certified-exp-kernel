# Validation Summary

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Current qualified candidate

The current benchmarked candidate is the **signed-window-6 (SW6) cached EXP kernel** for exact dyadic inputs. The final validation evidence is tied to this candidate rather than to the earlier direct-only or recovered-dyadic benchmark packages.

## Benchmark correctness gates

Every case in the final SW6-vs-FLINT campaign had to pass both of the following before its timing result was accepted:

1. exact-rounded agreement with the requested-precision MPFR reference;
2. independent higher-precision directed-MPFR certification.

Two deterministic campaigns were run at 10,000, 20,000 and 100,000 decimal digits:

- `0 < |a| < 1`: 100 positive + 100 negative inputs per precision;
- `1 < |a| < 100`: 100 positive + 100 negative inputs per precision.

Final correctness result:

- unit domain: **600 / 600 passed**;
- wide domain: **600 / 600 passed**;
- combined current benchmark set: **1,200 / 1,200 passed**;
- observed benchmark correctness failures: **0**.

## Final 10M endurance qualification

A separate persistent-process endurance test exercised the frozen SW6 candidate at **10,000 decimal digits** over a mixed exact-dyadic workload spanning:

- `+(0,1)`;
- `-(0,1)`;
- `+(1,100)`;
- `-(1,100)`.

The run used denominator `2^20`, 256 preflight inputs and deterministic seed `2026082203`.

Result:

- calls: **10,000,000 / 10,000,000 successful**;
- invalid outputs: **0**;
- preflight correctness: **256 / 256 passed**;
- sampled directed-MPFR checks: **10,000 / 10,000 passed**;
- correctness failures: **0**;
- SW6 cache setup: **16.192 ms**;
- production runtime: **1,231.122 s**;
- production throughput: **8,122.7 calls/s**;
- RSS start/end: **5,960 KB -> 6,168 KB**;
- RSS at 1M and 10M: **6,168 KB -> 6,168 KB**.

The memory checkpoints show the working set reaching a plateau by the 1M checkpoint and remaining unchanged through 10M calls in this run.

## What this evidence means

The current evidence supports the statement that, **within the documented tested regions**, the SW6 candidate completed a large repeated-call workload without observed execution or sampled correctness failure and without progressive RSS growth after warm-up.

The 10M endurance run is deliberately separate from the FLINT speed comparison. Its throughput should not be used as a FLINT benchmark number.

## What is not established by this qualification

The current qualification does not by itself establish:

- universal correctness for every possible exact dyadic;
- behavior on every CPU, OS or compiler;
- thread safety or multi-thread scaling;
- performance outside the documented benchmark regions;
- arbitrary rational denominators;
- production API compatibility for every downstream integration.

Those are separate integration and portability questions rather than unresolved results inside the completed SW6 benchmark/endurance campaign.

## Historical validation

Older direct-kernel and large-magnitude-extension campaigns remain part of the research record, including higher-precision containment work and earlier bug-finding validation. They are historical provenance, not the current headline qualification.

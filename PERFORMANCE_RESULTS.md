# Performance Results

> **DRAFT - FOR REVIEW BEFORE PUBLIC RELEASE**

This file records the current same-runner benchmark set for exact non-dyadic rational inputs.

All speedups are reported as:

`FLINT 3.6.0 arb_exp time / kernel time`

A value greater than 1 means the kernel was faster.

## Benchmark design

Two magnitude bands were tested at 10,000 and 20,000 decimal digits:

1. `0 < |x| < 1`;
2. `1 < |x| < 100`.

Each precision/domain block contained **100 deterministic random exact reduced non-dyadic rationals: 50 positive + 50 negative**. Within each sign, 25 denominators were odd and 25 were even but not powers of two. The input generators rejected non-reduced fractions and power-of-two denominators.

Seeds:

- unit band: `202608220901`;
- wide band: `202608220902`.

The benchmark generator used denominators in `[3, 2^31)` and machine-word signed numerators. Five timing repetitions per input and one thread were used. FLINT 3.6.0 was restored from the workflow cache and timed in the same GitHub Actions job as the kernel.

Correctness was a hard gate: every reported case passed both interval overlap and an independent higher-precision `arb_contains` containment check from the exact rational input.

## Results

| Domain | Digits | Mean kernel ms | Mean FLINT ms | Aggregate FLINT/kernel | Median ratio | Positive aggregate | Negative aggregate |
|---|---:|---:|---:|---:|---:|---:|---:|
| `0 < |x| < 1` | 10,000 | 0.810155 | 1.241056 | **1.5319x** | **1.5529x** | **1.5431x** | **1.5210x** |
| `0 < |x| < 1` | 20,000 | 2.060636 | 3.963175 | **1.9233x** | **1.9392x** | **1.9351x** | **1.9119x** |
| `1 < |x| < 100` | 10,000 | 0.866520 | 1.246729 | **1.4388x** | **1.4321x** | **1.4361x** | **1.4415x** |
| `1 < |x| < 100` | 20,000 | 1.207725 | 2.222107 | **1.8399x** | **1.8614x** | **1.8471x** | **1.8326x** |

Qualification totals:

- cases: **400**;
- wins vs FLINT: **400 / 400**;
- overlap checks: **400 / 400**;
- independent containment checks: **400 / 400**;
- observed benchmark correctness failures: **0**.

## Setup transparency

A one-time setup is performed for each fixed-precision benchmark run. Headline per-input timings exclude that one-time cost. The measured setup and the conservative 100-call batch ratio after charging one full setup are:

| Domain | Digits | One-time setup ms | FLINT/kernel including one setup over 100 calls |
|---|---:|---:|---:|
| `0 < |x| < 1` | 10,000 | 23.098788 | **1.1920x** |
| `0 < |x| < 1` | 20,000 | 57.381607 | **1.5044x** |
| `1 < |x| < 100` | 10,000 | 23.003177 | **1.1370x** |
| `1 < |x| < 100` | 20,000 | 31.926928 | **1.4552x** |

The setup-charged ratio remains greater than 1 in all four blocks.

## Run references

- unit 10K: GitHub Actions run `32589611436`;
- unit 20K: run `32589679452`;
- wide 10K: run `32589803242`;
- wide 20K: run `32589953714`.

## Separate endurance evidence

The 10M endurance record at 10,000 digits used a different exact-input workload and harness. It is stability evidence, not part of the FLINT ratio table. See `VALIDATION_SUMMARY.md`.

## Disclosure boundary

Only observable benchmark behavior and methodology are reported here. **No internal method is disclosed.**

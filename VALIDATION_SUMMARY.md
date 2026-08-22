# Validation Summary

> **DRAFT - FOR REVIEW BEFORE PUBLIC RELEASE**

## Current non-dyadic benchmark qualification

The current comparative qualification consists of four 100-case exact-rational blocks:

- `0 < |x| < 1` at 10K digits;
- `0 < |x| < 1` at 20K digits;
- `1 < |x| < 100` at 10K digits;
- `1 < |x| < 100` at 20K digits.

Each block contains 50 positive and 50 negative reduced exact non-dyadic rationals.

For every case, the candidate result had to pass:

1. overlap with the same-input FLINT result at requested precision; and
2. independent higher-precision containment checking from the exact rational input.

Current result:

- **400 / 400 overlap checks passed**;
- **400 / 400 containment checks passed**;
- **400 / 400 cases were faster than FLINT 3.6.0** in their same-runner per-input comparisons;
- observed correctness failures: **0**.

## Separate 10M endurance qualification

A separate previously frozen evaluator completed a persistent-process endurance test at **10,000 decimal digits** using a deterministic exact-dyadic mixed workload across positive/negative unit and wide bands.

Result:

- calls: **10,000,000 / 10,000,000 successful**;
- invalid outputs: **0**;
- preflight correctness: **256 / 256 passed**;
- sampled directed-MPFR checks: **10,000 / 10,000 passed**;
- correctness failures: **0**;
- production runtime: **1,231.122 s**;
- throughput: **8,122.7 calls/s**;
- RSS start/end: **5,960 KB -> 6,168 KB**;
- RSS at 1M and 10M: **6,168 KB -> 6,168 KB**.

The endurance workload is deliberately not relabeled as non-dyadic qualification.

## What the current evidence supports

Within the documented benchmark bands and precisions, the current exact-rational candidate showed:

- complete success on the reported correctness gates;
- 400/400 per-input wins against same-runner FLINT 3.6.0;
- closely matched positive and negative aggregate behavior.

The endurance evidence separately supports long-run stability for the stated 10K exact-dyadic workload.

## What remains outside this qualification

The current evidence does not by itself establish:

- universal correctness for every rational number and every height;
- arbitrary-height numerator/denominator API qualification;
- behavior on every CPU, OS or compiler;
- thread safety or multi-thread scaling;
- comparative performance outside `|x| < 100`;
- generic-rational 10M endurance;
- production API compatibility for every downstream integration.

## Disclosure boundary

This public validation record contains no internal method.

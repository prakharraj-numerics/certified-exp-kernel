# Public Technical Overview

> **DRAFT - FOR REVIEW BEFORE PUBLIC RELEASE**

## Purpose

This project evaluates a high-precision kernel for `exp(x)` on exact rational inputs, including non-dyadic rationals.

This public overview intentionally describes only measurable behavior, validation, tested scope and integration boundaries. **No internal method is disclosed.**

## Current tested operating region

The current mixed-sign non-dyadic benchmark campaign covers:

- exact reduced non-dyadic rational inputs;
- both signs;
- `0 < |x| < 1` and `1 < |x| < 100`;
- 10,000 and 20,000 decimal digits;
- one thread;
- 100 cases per domain/precision block, split 50 positive + 50 negative.

## Performance result

Against same-runner FLINT 3.6.0 `arb_exp`, aggregate FLINT/kernel ratios were:

- unit band, 10K: **1.5319x**;
- unit band, 20K: **1.9233x**;
- wide band, 10K: **1.4388x**;
- wide band, 20K: **1.8399x**.

All 400 benchmark cases were individual wins for the kernel.

See `PERFORMANCE_RESULTS.md` for sign splits, means, median ratios and setup accounting.

## Correctness evidence

Every reported case passed both overlap and independent higher-precision containment checking.

**400 / 400 cases passed both gates.**

## Endurance evidence

A separate exact-dyadic evaluator completed **10,000,000 / 10,000,000 calls at 10K digits**, with 0 invalid outputs, 10,000 / 10,000 sampled directed-MPFR checks passed, and no RSS growth from the 1M checkpoint through 10M.

That is a separate stability qualification and is not generic-rational endurance evidence.

## Input-height qualification boundary

The current non-dyadic benchmark generator used denominators below `2^31` and machine-word signed numerators. Broader exact-rational height remains a separate qualification dimension.

## Reference comparator

The current primary comparator is **FLINT 3.6.0 `arb_exp`**. Each benchmark block timed both implementations on the same GitHub-hosted Ubuntu 24.04 job.

## Maturity

Current evidence is strong for the documented high-precision single-threaded ranges, while cross-platform reproduction, customer-specific workloads, arbitrary-height API qualification, multi-thread behavior and production integration remain separate activities.

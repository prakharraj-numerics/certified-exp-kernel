# Public Technical Overview

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Purpose

This project evaluates a specialized numerical kernel for computing the exponential function on **exact-rational inputs** at very high requested precision.

The implementation returns a certified interval containing the true value rather than only an unqualified point estimate.

## Current canonical scope

The current canonical executable is the **rigorous direct kernel**.

It does not yet include the later large-magnitude reduction/dispatch layer explored in the broader research campaign. Consequently, large input magnitude remains a known performance weakness of the current direct release even when correctness/certification succeeds.

The present input front end also uses machine-word-sized numerator/denominator components (approximately 64-bit). That rational-height/interface ceiling is separate from the large-magnitude issue.

## Intended use

The current prototype is aimed at workloads where:

- the input is known exactly as a rational number;
- very high precision is required;
- rigorous numerical certification matters;
- numerator and denominator fit comfortably within the present machine-word interface;
- a specialized fast path can be selected instead of a fully general exponential routine.

Pending integration of the later magnitude-reduction layer, the current direct release is best viewed as a fast path for moderate-magnitude exact-rational inputs rather than a uniformly optimized path across very large `|a|`.

## Reference implementation

The primary benchmark comparator is FLINT 3.6.0 `arb_exp`.
FLINT 3.0.1 was also used as a historical secondary comparator, with directionally consistent results.

## Evidence layers

The repository distinguishes between:

1. **current canonical direct-release behavior**, and
2. **broader historical research-campaign results**, including a later large-magnitude dispatcher.

Historical dispatcher results remain useful evidence that the large-magnitude weakness can be addressed, but they are not capabilities of the current direct-only executable until that layer is integrated and revalidated in the current source line.

## Current maturity

The software is a research prototype.

It has undergone broad correctness and performance testing, but the present evidence does not establish:

- cross-machine portability of the speedup;
- universal superiority at low precision;
- uniformly favorable performance for large argument magnitude in the current direct-only release;
- support for arbitrarily large numerator/denominator bit lengths;
- production API stability;
- complete adversarial coverage.

## Confidentiality boundary

This public description intentionally discusses **capability, validation, release boundaries and measured behavior only**.

It does not describe the formula, derivation, series identity, internal algorithm, implementation architecture, or optimization path.

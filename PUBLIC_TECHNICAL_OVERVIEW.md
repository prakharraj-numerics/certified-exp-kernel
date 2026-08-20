# Public Technical Overview

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Purpose

This project evaluates a specialized numerical kernel for computing the exponential
function on **exact-rational inputs** at very high requested precision.

The implementation returns a certified interval containing the true value rather than
only an unqualified point estimate.

## Intended use

The current prototype is aimed at workloads where:

- the input is known exactly as a rational number;
- very high precision is required;
- rigorous numerical certification matters;
- a specialized fast path can be selected instead of a fully general exponential routine.

## Reference implementation

The primary benchmark comparator is FLINT 3.6.0 `arb_exp`.
FLINT 3.0.1 was also used as a historical secondary comparator, with directionally
consistent results.

## Current maturity

The software is a research prototype.

It has undergone broad correctness and performance testing, but the present evidence
does not establish:

- cross-machine portability of the speedup;
- universal superiority at low precision;
- support for arbitrarily large numerator/denominator bit lengths;
- production API stability;
- complete adversarial coverage.

## Confidentiality boundary

This public description intentionally discusses **capability, validation and measured
behavior only**.

It does not describe the formula, derivation, series identity, internal algorithm,
implementation architecture, or optimization path.

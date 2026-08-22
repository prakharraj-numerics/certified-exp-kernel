# Supported Scope and Limitations

> **DRAFT - FOR REVIEW BEFORE PUBLIC RELEASE**

## Mathematical input form

The current kernel program targets exact rational inputs

`x = p / q`, with `q > 0`,

including genuine non-dyadic rationals.

Both positive and negative arguments are covered by the current comparative qualification.

## Current benchmarked scope

Current same-runner evidence against FLINT 3.6.0 covers:

- `0 < |x| < 1`;
- `1 < |x| < 100`;
- 10,000 and 20,000 decimal digits;
- 100 exact non-dyadic rationals per domain/precision block;
- 50 positive + 50 negative per block;
- one thread;
- five timing repetitions per input.

Across these four blocks, **400 / 400 cases passed both reported correctness gates and 400 / 400 were faster than FLINT**.

## Current benchmark input-height envelope

The current qualification generator used:

- positive denominators in `[3, 2^31)`;
- reduced fractions only;
- non-power-of-two denominators only;
- machine-word signed numerators generated to satisfy the target magnitude band.

This is the current published benchmark envelope. It should not be confused with the broader mathematical definition of an exact rational input.

## Precision

The current mixed-sign non-dyadic headline qualification is at **10K and 20K decimal digits**.

Earlier specialized exact-input work includes higher-precision evidence, but it is not mixed into the current generic-rational headline table.

## Separate endurance-tested scope

The existing 10M endurance qualification used exact dyadic inputs at 10,000 digits. It completed 10,000,000 / 10,000,000 calls with zero invalid outputs and zero sampled correctness failures. That result remains useful stability evidence but is not generic-rational endurance qualification.

## Setup and repeated-call behavior

Current benchmark reporting separates one-time run setup from repeated per-input timing and additionally reports a 100-call ratio with one complete setup charged to the batch.

## Current non-goals

This prototype should not be represented as:

- a universal replacement for every exponential implementation;
- a proof covering every exact rational height;
- a cross-platform performance guarantee;
- a multi-thread scaling result;
- a comparative performance claim outside the documented magnitude and precision bands;
- a claim that the existing 10M endurance run used non-dyadic rationals.

## Disclosure boundary

The public scope document discloses no internal mathematical or implementation method.

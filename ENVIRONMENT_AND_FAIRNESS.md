# Benchmark Environment and Fairness

> **DRAFT - FOR REVIEW BEFORE PUBLIC RELEASE**

## Environment

The current mixed-sign non-dyadic comparisons were executed in GitHub Actions on:

- runner image: **Ubuntu 24.04**;
- architecture: **x86-64**;
- execution: **single-threaded**;
- kernel build profile: `-O3 -march=x86-64-v2 -DNDEBUG`;
- comparator: **FLINT 3.6.0 `arb_exp`**;
- timing repetitions: **5 per input**;
- GMP/MPFR supplied by the runner environment;
- FLINT 3.6.0 restored from the established workflow cache.

Kernel and FLINT were timed in the **same hosted-runner job** for each block. Because hosted-runner absolute times can vary, the same-job FLINT/kernel ratio is the primary comparative quantity.

## Workloads

### `0 < |x| < 1`

- precision: 10K and 20K decimal digits;
- 100 exact reduced non-dyadic rationals per precision;
- 50 positive + 50 negative;
- per sign: 25 odd denominators + 25 even non-power-of-two denominators;
- seed: `202608220901`.

### `1 < |x| < 100`

- precision: 10K and 20K decimal digits;
- 100 exact reduced non-dyadic rationals per precision;
- 50 positive + 50 negative;
- per sign: 25 odd denominators + 25 even non-power-of-two denominators;
- seed: `202608220902`.

Denominators were sampled below `2^31`, fractions were reduced, and power-of-two denominators were rejected.

## Timing fairness

For each exact input, both implementations received the same rational value and requested precision. Input parsing/construction was outside the repeated function timing. The median of five calls was used for each implementation.

A one-time kernel setup is reported separately. `PERFORMANCE_RESULTS.md` provides both repeated-call ratios and a conservative 100-call ratio after charging one complete setup to the batch.

## Correctness gate

Every accepted benchmark case passed:

- interval overlap with FLINT at requested precision; and
- independent higher-precision containment checking from the exact rational input.

Current total: **400 / 400 cases passed both gates**.

## Hosted-runner interpretation

GitHub-hosted runners are virtualized and may differ in physical CPU assignment, frequency behavior and contention. Therefore:

- do not infer a regression from small absolute timing changes across separate runs without a same-job comparator;
- use same-job FLINT/kernel ratios for performance claims;
- keep endurance throughput separate from comparative benchmark ratios.

## Separate endurance environment

The 10M endurance record was also produced on GitHub-hosted Ubuntu 24.04 in one persistent process at 10K digits. Its workload used exact dyadic inputs and is reported only as stability evidence.

## Disclosure boundary

No internal method is described in this public benchmark record.

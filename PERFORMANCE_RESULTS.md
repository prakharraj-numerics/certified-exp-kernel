# Performance Results

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

All ratios below are:

`prototype wall-clock time / FLINT wall-clock time`

A ratio below 1.0 means the prototype was faster.

## Precision scaling

A fixed representative set of six values was tested across precision.

| Decimal digits | Geometric-mean ratio | Median ratio | Win rate |
|---:|---:|---:|---:|
| 1,000 | 0.82 | 0.89 | 4 / 6 |
| 2,000 | 0.75 | 0.77 | 5 / 6 |
| 5,000 | 0.61 | 0.64 | 6 / 6 |
| 10,000 | 0.47 | 0.49 | 6 / 6 |
| 20,000 | 0.38 | 0.41 | 6 / 6 |
| 50,000 | 0.36 | 0.37 | 6 / 6 |
| 100,000 | 0.36 | 0.33 | 6 / 6 |
| 200,000 | 0.33 | 0.29 | 6 / 6 |

The documented campaign also tested 500,000 decimal digits and reported that the
high-precision performance plateau continued rather than collapsing.

## Random exact-rational stress test

At 100,000 decimal digits, the original direct-path campaign tested 400 fixed-seed
random exact rationals, split evenly across four sign/magnitude strata.

Against FLINT 3.6.0:

- `0 < a < 1`: 100 / 100 wins; median ratio 0.25
- `-1 < a < 0`: 100 / 100 wins; median ratio 0.29
- `a > 1`: 99 / 100 wins; median ratio 0.60
- `a < -1`: 100 / 100 wins; median ratio 0.61

Total: **399 / 400 wins**, with zero correctness failures.

A later extension was tested specifically on the difficult large-magnitude cases.
A final simple dispatcher was then validated over the original 400-case set plus
12 additional integer cases spanning magnitudes from 1 to 10,000, both signs:

**411 / 412 wins, zero correctness failures.**

## Large-magnitude results

The extended path was tested on integer magnitudes from 1 through 10,000, both signs.
Every tested integer case won.

A separate stratified set of 36 large-magnitude noninteger exact rationals, with
magnitudes from 10 to 9,000 and both signs, converted all 8 direct-path losses in that
set to wins under the extended path.

A different retest, drawn from the earlier random campaign, contained one residual
noninteger case with a power-of-two denominator that remained slower than FLINT. That
case is documented separately in `SUPPORTED_SCOPE.md`.

## Cold and warm behavior

For the direct path, cold and warm evaluation costs are close: documented one-time
configuration cost was only about 4% of a representative 100,000-digit evaluation.

The extended path can reuse a precision-specific intermediate across repeated queries.
In one representative 10-query, 100,000-digit integer batch:

- one-time reusable computation: approximately 0.0068 s
- cold per-query cost: approximately 0.0134 s
- warm per-query cost: approximately 0.0049 s

This is a measured workload-specific result, not a universal throughput guarantee.

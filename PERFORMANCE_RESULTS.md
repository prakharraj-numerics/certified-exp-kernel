# Performance Results

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

All ratios below are:

`prototype wall-clock time / FLINT wall-clock time`

A ratio below 1.0 means the prototype was faster.

## Release-boundary note

The **current canonical executable is the rigorous direct kernel only**. It does not yet include the later large-magnitude reduction/dispatch layer.

Therefore, the direct-kernel measurements below remain directly relevant to the current release line, while the later dispatcher and large-magnitude tables are **historical research-campaign evidence** and must not be presented as measurements of the current direct-only executable.

A current-build 5,000-digit smoke case at `a = 100` passed higher-precision Arb containment but had `ours / FLINT = 1.1874`, illustrating the known large-magnitude performance weakness of the present direct release.

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

The documented campaign also tested 500,000 decimal digits and reported that the high-precision performance plateau continued rather than collapsing.

## Direct-kernel random exact-rational stress test

At 100,000 decimal digits, the original direct-path campaign tested 400 fixed-seed random exact rationals, split evenly across four sign/magnitude strata.

Against FLINT 3.6.0:

- `0 < a < 1`: 100 / 100 wins; median ratio 0.25
- `-1 < a < 0`: 100 / 100 wins; median ratio 0.29
- `a > 1`: 99 / 100 wins; median ratio 0.60
- `a < -1`: 100 / 100 wins; median ratio 0.61

Total: **399 / 400 wins**, with zero correctness failures in that recorded campaign.

These are direct-kernel results and are the closest broad historical performance layer to the current canonical direct release. They were produced in the documented historical environment, not by the currently packaged binary.

## Historical large-magnitude extension

A later extension was tested specifically on difficult large-magnitude cases. A final simple dispatcher was then validated over the original 400-case set plus 12 additional integer cases spanning magnitudes from 1 to 10,000, both signs:

**411 / 412 wins, zero correctness failures.**

This **411 / 412** result belongs to the broader historical research campaign. The dispatcher responsible for it is **not present in the current canonical direct executable**.

The extended path was tested on integer magnitudes from 1 through 10,000, both signs, and every tested integer case won in that campaign.

A separate stratified set of 36 large-magnitude noninteger exact rationals, with magnitudes from 10 to 9,000 and both signs, converted all 8 direct-path losses in that set to wins under the extended path.

A different retest, drawn from the earlier random campaign, contained one residual noninteger case with a power-of-two denominator that remained slower than FLINT. That historical weak subclass is documented separately in `SUPPORTED_SCOPE.md`.

## Cold and warm behavior

For the historical direct path, cold and warm evaluation costs were close: documented one-time configuration cost was only about 4% of a representative 100,000-digit evaluation.

The historical extended path could reuse a precision-specific intermediate across repeated queries. In one representative 10-query, 100,000-digit integer batch:

- one-time reusable computation: approximately 0.0068 s
- cold per-query cost: approximately 0.0134 s
- warm per-query cost: approximately 0.0049 s

This is a measured historical workload-specific result, not a universal throughput guarantee and not a capability claim for the present direct-only canonical executable.

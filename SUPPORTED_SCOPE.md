# Supported Scope and Limitations

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Current supported input form

The current canonical release accepts an exact rational input represented by an integer numerator and denominator.

The present interface is limited to approximately **64-bit numerator/denominator components**. Inputs beyond that capacity are not supported by this version.

This is a separate limitation from large argument magnitude. A value can have small rational height but large magnitude, or large rational height while remaining numerically close to zero.

## Precision

Tested requested precision in the broader research campaign:

**1,000 to 500,000 decimal digits**

The speed advantage is not universal at the low end. The documented representative precision-scaling set became uniformly favorable from approximately 5,000 digits upward.

## Large argument magnitude

The **current canonical executable is the direct kernel only**. It does not yet include the later large-magnitude reduction/dispatch layer from the broader research campaign.

Accordingly, large `|a|` is a known **performance weakness** of the current release. This is not presented as a correctness defect: a current-build 5,000-digit smoke test at `a = 100` passed higher-precision Arb containment but was slower than FLINT (`ours / FLINT = 1.1874`).

The broader historical research campaign did validate a later magnitude-reduction strategy on integer magnitudes through 10,000 and large noninteger values into the several-thousand range. Those results remain historical evidence and should not be attributed to the present direct-only executable.

## Rational height

Performance degrades as numerator/denominator bit length increases.

For a tested value near 0.5 in the documented campaign, the timing ratio progressed from approximately 0.31 at 8-bit height to 1.19 at 64-bit height.

Accordingly, the current fast path is most attractive for **low-height exact rationals**, and the present interface should not be described as an arbitrary-height rational front end.

## Narrow historical residual subclass

In the broader large-magnitude research campaign, one documented noninteger case with a denominator that is itself an exact power of two did not benefit from the large-magnitude extension and remained slower than the reference implementation.

Matched-magnitude testing showed that FLINT itself was substantially faster on that power-of-two-denominator input than on a nearby ordinary-denominator control, while the prototype's own timing changed little. The internal reason for FLINT's advantage was not established.

This remains a useful historical weak-class observation, but the current direct-only release already has the more general large-magnitude limitation described above.

## Current non-goals

This public prototype should not be represented as:

- a universal replacement for `arb_exp`;
- universally faster below a few thousand digits;
- uniformly faster for large argument magnitude in the current direct-only release;
- validated on all processor architectures;
- validated for arbitrary-size rational numerators and denominators;
- production-hardened software.

# Supported Scope and Limitations

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Current supported input form

The current implementation accepts an exact rational input represented by an integer
numerator and denominator.

The present build is limited to approximately **64-bit numerator/denominator
representation** internally. Inputs beyond that capacity are not supported by this
version.

## Precision

Tested requested precision:

**1,000 to 500,000 decimal digits**

The speed advantage is not universal at the low end. The documented representative
precision-scaling set became uniformly favorable from approximately 5,000 digits upward.

## Magnitude and sign

Validated classes include:

- positive and negative fractions below magnitude 1;
- positive and negative values above magnitude 1;
- integers from magnitude 1 through 10,000;
- large noninteger exact rationals into the several-thousand range.

## Rational height

Performance degrades as numerator/denominator bit length increases.

For a tested value near 0.5, the documented timing ratio progressed from approximately
0.31 at 8-bit height to 1.19 at 64-bit height.

Accordingly, the current fast path is most attractive for **low-height exact rationals**.

## Narrow residual subclass

One documented noninteger case with a denominator that is itself an exact power of two
did not benefit from the large-magnitude extension and remained slower than the
reference implementation.

Matched-magnitude testing showed that FLINT itself was substantially faster on that
power-of-two-denominator input than on a nearby ordinary-denominator control, while
the prototype's own timing changed little. The internal reason for FLINT's advantage
was not established. This remains a known residual limitation.

## Current non-goals

This public prototype should not be represented as:

- a universal replacement for `arb_exp`;
- universally faster below a few thousand digits;
- validated on all processor architectures;
- validated for arbitrary-size rational numerators and denominators;
- production-hardened software.

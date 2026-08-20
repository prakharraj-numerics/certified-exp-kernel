# Validation Summary

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Correctness checks used

Reported benchmark cases were checked for:

1. agreement/overlap with the reference library result;
2. requested relative accuracy;
3. rigorous interval certification.

The broader research campaign reports more than 1,000 correctness-checked evaluations.

## Independent verification

A stronger verification pass recomputed selected cases from scratch at higher precision
and checked that the lower-precision certified result contained the tighter recomputation.

For the direct kernel:

- **40 / 40 independent higher-precision containment checks passed**.

For the later large-magnitude extension, a dedicated rigor sweep exposed a
working-precision-margin problem during development. The affected results still
overlapped and contained the reference value, but some did not meet the requested
accuracy margin. The guard policy was corrected and the same 27-case sweep then
passed **27 / 27**.

This history is retained because it is evidence that validation was used to find real
defects rather than merely confirm expected results.

## Certification model

The public package does not disclose the internal derivation of the error bound.

The current evidence supports only the following public claim:

**the implementation returns a certified numerical interval and the documented
validation campaign observed zero correctness failures after the final fixes.**

## Remaining validation gaps documented in the research report

- cross-machine verification;
- microarchitectural profiling;
- a broader adversarial sweep, including additional unusual rational structures and
  bit-pattern cases.

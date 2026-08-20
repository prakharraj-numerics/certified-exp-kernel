# Validation Summary

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Correctness checks used

Reported benchmark cases in the broader research campaign were checked for:

1. agreement/overlap with the reference library result;
2. requested relative accuracy;
3. rigorous interval certification.

The broader research campaign reports more than 1,000 correctness-checked evaluations.

## Current canonical direct release

The current canonical executable is the **rigorous direct kernel only**. Its evaluation driver performs a higher-precision Arb recomputation and checks containment with `arb_contains`.

A current-build smoke gate tested `1/3`, `13/10`, `-10`, and `100` at 5,000 decimal digits. All four returned successful containment. The `a = 100` case was deliberately retained even though the direct kernel was slower than FLINT, because large argument magnitude is a known performance weakness of the current direct-only release.

## Independent verification in the documented direct campaign

A stronger verification pass recomputed selected direct-kernel cases from scratch at higher precision and checked that the lower-precision certified result contained the tighter recomputation.

For the documented direct kernel:

- **40 / 40 higher-precision containment checks passed**.

## Historical large-magnitude extension

A later large-magnitude extension was developed and validated in the broader research campaign, but it is **not included in the current canonical direct executable**.

During development, a dedicated rigor sweep exposed a working-precision-margin problem. The affected results still overlapped and contained the reference value, but some did not meet the requested accuracy margin. The guard policy was corrected and the same 27-case sweep then passed **27 / 27**.

This history is retained because it is evidence that validation was used to find real defects rather than merely confirm expected results. It should not be read as a statement that the later extension is already integrated into the current public release line.

## Certification model

The public package does not disclose the internal derivation of the error bound.

The current evidence supports only the following public claim:

**the direct kernel returns a certified numerical interval, and the documented validation campaigns observed zero correctness failures after the recorded fixes.**

## Remaining validation and integration gaps

- integration of the validated large-magnitude strategy into the current canonical source line;
- cross-machine verification;
- microarchitectural profiling;
- a broader adversarial sweep, including additional unusual rational structures and bit-pattern cases;
- a wider-than-machine-word rational input front end.

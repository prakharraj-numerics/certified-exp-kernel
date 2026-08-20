# Benchmark Environment and Fairness

> **DRAFT — FOR REVIEW BEFORE PUBLIC RELEASE**

## Documented environment

- execution: single-threaded
- CPU: Intel Xeon, virtualized, 2.10 GHz
- measured cores: 1
- available RAM: 3.9 GiB
- operating system: Linux, kernel 6.18
- compiler: g++ 13.3.0
- prototype build flags: `-O3 -march=native -DNDEBUG`
- GMP: 6.3.0
- MPFR: 4.2.1
- primary reference: FLINT 3.6.0
- secondary historical reference: FLINT 3.0.1

## Comparator choice

The general-purpose FLINT exponential routine was used as the reference.

The research campaign checked the public FLINT interface for a stronger public
exact-rational-specialized exponential entry point and found none. The general
`arb_exp` entry point was therefore used as the practical public comparator.

## Timing protocol

The documented benchmark methodology used:

- identical requested precision;
- alternating prototype/reference execution order to reduce systematic timing drift;
- multiple repetitions;
- median reporting;
- separately disclosed setup/amortization effects where relevant.

## Important limits of the current evidence

The results have **not** yet been confirmed on a second physical machine or a second
microarchitecture.

No cycle/instruction-level profiling was performed in the documented campaign.

The report records the prototype compiler flags and the FLINT versions used, but does
not establish that the FLINT build used identical compiler/CPU optimization flags.
Accordingly, this public package does not claim build-flag equivalence between the two
implementations.

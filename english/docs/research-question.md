# Research Question

The project tests whether partial, noisy information about the ECDSA
nonce can be transformed, in a controlled experiment, into sufficient
information for verifiable recovery of the private key.

The primary computational question is:

> For team-generated ECDSA signatures over secp256k1, how much nonce
> information must be extracted from synthetic traces for a CNN to
> produce sufficiently accurate prefixes and for HNP & LLL to recover
> the private key?

The base experiment fixes the nonce's first 12 MSB bits, uses 40
signatures for the HNP instance, and retains an explicit split of the
160 samples: 96 train, 24 validation, and 40 test/attack.

The project separates three verification levels:

1. correctness of ECDSA and the HNP equations;
2. lattice reduction through LLL and candidate validation;
3. key recovery using prefixes produced by the CNN.

The oracle instance uses known prefixes to test the HNP solver without
the error introduced by the CNN. This check shows whether the lattice
problem can be solved under the given conditions, but it does not
demonstrate that the CNN can produce the same prefixes.

For the CNN portion, the benchmark varies `ell`, `sigma`, and `m` and
separates prediction accuracy from HNP success. Thus, a classification
error is not confused with an error by the lattice solver.

The mathematical portion has an explicit limit. The rigorous result is
formulated for the scalar case with three families and the associated
exponent transfer. Extension to simultaneous approximation for more
than three families remains uncovered by the current proof.

The hardware stage remains separate from the current results.
The repository retains the ESP32 firmware and protocol, but the
project does not present physical measurements until they exist.


## Leakage Model and Current Status

The default model in `ecdsa_leakage_model4.py` is now a synthetic
temporal model with two-position memory, not the prefix HW model that
allowed the exact identity
`HW(prefix_t) - HW(prefix_{t-1}) = b_t`. The temporal model is a
research abstraction, not a validated model of ESP32 power consumption.
Differences between samples may still contain information about the
label, so simple baselines and evaluation on independent data must be
reported.

Historical results generated with the HW model are retained for
traceability, but must not be cited as results from the current default
configuration. Regenerate the profile dataset, CNN training, predictions,
and all benchmarks before reporting new metrics.

The computer science contribution evaluated here is experimental:
a comparison among a synthetic temporal model, bit/prefix classification,
and HNP/LLL solving under controlled conditions.

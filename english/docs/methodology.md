# Methodology

The methodology separates three components that can fail independently: algebraic validation, CNN inference, and lattice recovery. Each stage uses identifiable artifacts and has its own verification criterion.

## ECDSA and HNP

For each signature, calculate the terms used in the HNP formulation:

```text
t_i = s_i^(-1) r_i mod n
u_i = s_i^(-1) z_i mod n
a_i = k_bar_i 2^(256-ell)
```

The HNP equation used by the solver is:

```text
t_i d + u_i - a_i = delta_i (mod n)
```

where:

```text
0 <= delta_i < B
B = 2^(256-ell)
```

The lattice has dimension `m + 2`. `HNPLatticeSolver` constructs the embedding and verifies the expected vector separately. The `solve()` method reduces the basis, searches for the compatible vector, and validates the candidate obtained.

## CNN

`NonceBitCNN` receives two channels for the raw trace and the difference between samples. The default trace-generation model is temporal and synthetic: each sample combines the current bit with two previous bits and Gaussian noise. It is not a physically validated simulation of the ESP32. The prefix HW model remains available only for historical comparisons; in that model, trace differentiation could reveal the bit directly and therefore must not be used to support conclusions about side-channel generalization. For the base configuration, the network predicts the 12 bits of the nonce MSB prefix.

The explicit split ensures that notebooks and production scripts use the same IDs for train, validation, and test. The 40 test IDs are then passed to the HNP stage.

## CNN → HNP

The CNN does not recover the private key directly. It provides partial nonce information, and this information is transformed into HNP constraints. The lattice solver uses the constraints to search for the candidate private key.

To separate errors, the project also runs an oracle instance. In the oracle, the nonce prefixes are known, allowing lattice performance to be measured without CNN uncertainty.

## Mathematical Results

`bridge_results.json` contains four numerical checks, and `diophantine_results.json` contains five more. The documentation keeps these two groups separate from the HNP attack results.

The rigorous mathematical part covers the scalar concurrence case for three families and the associated exponent transfer. Experiments for additional families can support numerical exploration, but they do not turn that exploration into a theorem for simultaneous approximation with `k > 3`.

## Interpretation Criteria

`VALIDATED` in the HNP stage means that the equations and bounds are compatible with the known key. It does not by itself mean that the attack recovered the key.

HNP `PASS` requires a candidate returned by LLL that passes complete validation and, for the reference execution, is obtained with the `fpylll` backend.

For benchmarks, a single run is not sufficient for a success rate. The `(ell, sigma, m)` matrix is run across multiple seeds and reported through the number of attempts and the number of recoveries. The results separately include HNP oracle and CNN → HNP.

# Project Overview

The project links two directions that meet in the same problem: the computer science component builds a controlled ECDSA experiment, while the mathematics component studies arithmetic concurrence and Diophantine approximation.

## Computer Science Chain

`dataset_generator3.py` generates keys, nonces, and ECDSA signatures over secp256k1. The public dataset contains only data available to the attack. In the controlled local environment, the oracle retains the private key and complete nonces for experimental verification; the public copy redacts the private key.

`ecdsa_leakage_model4.py` transforms the nonces into a synthetic model based on Hamming Weight / Hamming Distance and adds Gaussian noise. The base configuration uses `sigma = 0.15`, 256 samples per trace, and the nonce's first 12 MSB bits as the CNN target.

`cnn/train.py` and `cnn_nonce_analysis5.py` use the same explicit split: 96 samples for train, 24 for validation, and 40 for test/attack. The CNN reports validation results and produces exactly the 40 prefixes used by the HNP stage.

`hnp_attack1.py` constructs the HNP terms and runs LLL through the selected backend. `fpylll` is the reference backend, and `--require-fpylll` rejects the SymPy fallback. During development, SymPy can confirm the CNN → HNP chain, but the result is marked `PASS_DEVELOPMENT` and is not presented as a reference execution.

## Experimental Status

Existing mathematical validation is divided into two groups of artifacts. `bridge_results.json` contains four numerical checks, while `diophantine_results.json` contains five more. These results separately document the mathematical checks and are not confused with key recovery through the HNP attack.

Reference-run artifacts record `PASS` for the CNN → HNP chain on the base synthetic instance, using the `fpylll` backend and with the key verified against ground truth. This status describes that instance. The separate sweep aggregates 100 seeds for each of 20 configurations; the CNN → HNP recovery rate varies with the number of bits and noise level. The values are retained in `results/synthetic/HW/tables/cnn-hnp-parameter-sweep-summary.csv` and summarized in `project-summary.md`.

## Quantitative Experiments

`informatica/experiments/benchmark_cnn_hnp_surface.py` evaluates CNN accuracy and HNP recovery for 20 configurations. The available sweep uses 100 seeds, from `1000` through `1099`, 100 epochs, `ell` in `{8, 10, 12, 14}`, `sigma` in `{0, 0.05, 0.10, 0.15, 0.20}`, and `m = 40`.

The detailed table is `results/synthetic/HW/tables/cnn-hnp-parameter-sweep.csv`, the aggregated summary is `results/synthetic/HW/tables/cnn-hnp-parameter-sweep-summary.csv`, and the oracle thresholds are in `results/synthetic/shared/tables/hnp-oracle-thresholds.csv`.

## Mathematical Limit

The rigorous mathematical portion covers the scalar case with three families and the associated exponent transfer. The code contains experiments for additional families, but these must not be presented as a rigorous proof of the general case of simultaneous approximation for `k > 3`.

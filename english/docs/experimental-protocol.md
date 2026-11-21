# Experimental Protocol

## Base Configuration

The base dataset contains 160 ECDSA signatures over secp256k1. The generator uses seed `20260919`, fixes `ell = 12` MSB bits of the nonce, and constructs synthetic traces with 256 positions. The CNN profile uses a Hamming Weight-based model and Gaussian noise with `sigma = 0.15`.

The CNN runs for 100 epochs with a learning rate of `0.005`. The model retains the state with the highest validation accuracy.

## Split

The file `informatica/artifacts/dataset_split.json` is the sole source for the data split.

| split | samples | role |
| --- | ---: | --- |
| train | 96 | CNN training |
| validation | 24 | evaluation and model selection |
| test | 40 | final evaluation and HNP input |

No second implicit split is generated. The CNN produces exactly 40 predictions for the set used by HNP.

## Experiment Order

1. Generate the 160 signatures and validate the ECDSA relations.
2. Generate synthetic traces for the same IDs.
3. Train the CNN only on the `train` set.
4. Measure the validation and test results, then write the prefixes for the 40 test samples.
5. Construct the HNP instance from the public data and the prefixes produced by the CNN.
6. Run LLL with the `fpylll` backend in the reference execution and validate the candidate against the HNP relations.
7. Run the oracle instance separately to measure the lattice's capacity without CNN error.

## Success Criterion

An HNP recovery receives `PASS` only when the solver returns the correct key, the candidate passes the HNP bounds, and the reference execution uses the `fpylll` backend.

When the same verification is performed with SymPy in the development environment, the result is labeled `PASS_DEVELOPMENT`. This label preserves the distinction between local validation and reference execution.

## Quantitative Matrix

The file `informatica/experiments/benchmark_cnn_hnp_surface.py` produced a sweep with `ell` in `{8, 10, 12, 14}`, `sigma` in `{0, 0.05, 0.10, 0.15, 0.20}`, `m = 40`, and 100 seeds from `1000` through `1099`. Each configuration runs for 100 epochs.

Detailed results are in `results/synthetic/HW/tables/cnn-hnp-parameter-sweep.csv`, and aggregations over the 20 configurations are in `results/synthetic/HW/tables/cnn-hnp-parameter-sweep-summary.csv`. Oracle results are in `results/synthetic/shared/tables/hnp-oracle-thresholds.csv`, with no private-key values published.

Recovery rates differ between configurations. A false recovery result means that the configuration did not recover the key, not that the software test of the pipeline failed.

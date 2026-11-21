# Reproducibility

Project reproducibility is based on the same data, the same splits,
the same configurations, and the same LLL backend when the reference
experiment is executed.

## Recommended Environment

Project dependencies are declared in `requirements.txt`. The list
includes `fpylll`; to import this library completely, the environment
must also have `cysignals` available.

The basic installation is:

```bash
python -m pip install -r requirements.txt
```

The tests can then be run:

```bash
python -m pytest -v
```

During development, `main.py` can use SymPy when `fpylll` is
unavailable. Such a result is labeled `PASS_DEVELOPMENT`.

The reference execution must reject the fallback:

```bash
python informatica/ECDSA/main.py --require-fpylll
```

This command must run with the `fpylll` backend and verify the
CNN → HNP chain.

## LLL Backend

`HNPLatticeSolver.solve()` returns the candidate together with the
backend used. The `require_fpylll=True` option stops execution when
`fpylll` is unavailable, instead of automatically switching backends.

`hnp_attack1.py` writes the effective backend, backend policy, and
validation status to `hnp_result.json`. Thus, algebraic verification
performed with SymPy cannot accidentally be presented as a
reference execution with `fpylll`.

In the public archive, `informatica/artifacts/oracle_dataset.json`
retains complete test nonces, while the explicit private-key field is
redacted. Complete nonces together with signatures permit derivation
of the synthetic key, so this file is ground truth for evaluation,
not input data for a blind attack. The attack code must consume the
public dataset and predicted prefixes; the synthetic key is used only
after solver execution, during evaluation. Notebook 04 separately
reports the status of the recorded reference run and `NOT EXECUTED`
for the local rerun when the necessary data is unavailable. For a
complete rerun, execute
`python informatica/ECDSA/main.py --require-fpylll` in a controlled
local environment and do not publish locally generated oracle files
containing explicit secrets.

## Data and Split

The experiment uses 160 signatures and retains the same split in all stages:

- 96 train;
- 24 validation;
- 40 test/attack.

The file `informatica/artifacts/dataset_split.json` is the explicit
source for this split. The HNP stage uses exactly the 40 test examples.

## Quantitative Benchmark

The recorded sweep uses 100 seeds, from `1000` through `1099`,
100 epochs, `ell` in `{8, 10, 12, 14}`, `sigma` in
`{0, 0.05, 0.10, 0.15, 0.20}`, and `m = 40`. Existing CSVs are
historical results: the previous orchestrator version performed a
ground-truth key compatibility test before LLL, and timings for those
instances are not end-to-end. The current code no longer uses the
secret key to decide whether to run the solver. Before citing timings
for the entire sweep, regenerate the CSVs with the current script version.

To reproduce the saved configurations, run from the repository root:

```bash
python informatica/experiments/benchmark_cnn_hnp_surface.py \
  --leakage-mode HW \
  --leaked-bits 8 10 12 14 \
  --sigmas 0 0.05 0.10 0.15 0.20 \
  --sample-counts 40 \
  --seeds $(seq 1000 1099) \
  --epochs 100 \
  --output results/synthetic/HW/tables/cnn-hnp-parameter-sweep.csv
```

The reference execution requires `fpylll`. The command writes
`results/synthetic/HW/tables/cnn-hnp-parameter-sweep.csv` and the
aggregated summary
`results/synthetic/HW/tables/cnn-hnp-parameter-sweep-summary.csv`.

`results/synthetic/shared/tables/hnp-oracle-thresholds.csv` retains
the oracle verification for 400 combinations. Recovered private-key
values are redacted.

## CI

`.github/workflows/tests.yml` verifies the `fpylll` import, runs the
complete pipeline with `--require-fpylll`, and requires `hnp = PASS`,
`hnp_backend = fpylll`, `fully_executed = true`, and validated recovery
in the CNN-to-HNP chain.

`.github/workflows/reproducibility.yml` repeats the reference
verification. `.github/workflows/benchmark.yml` runs a short
exploratory sweep with three seeds and retains the results as a GitHub
Actions artifact; it does not replace the complete 100-seed
reproduction described above.

## Local Verification

For a development verification, you can run:

```bash
python informatica/ECDSA/main.py
```

If the environment does not have `fpylll`, the result may use SymPy
and must be interpreted as `PASS_DEVELOPMENT`.

For the verification required at submission, use:

```bash
python informatica/ECDSA/main.py --require-fpylll
```

and verify the resulting output in
`informatica/artifacts/hnp_result.json`.

## Temporal Results and Alternative Models

The temporal model and HD model now have separate directories at
`results/synthetic/TEMPORAL/` and `results/synthetic/HD/`. The
current archive does not include complete benchmark CSVs for these
two models, so we do not report them as obtained results. Benchmarks
can be regenerated separately:

```bash
python informatica/experiments/benchmark_cnn_hnp_surface.py --leakage-mode HD
python informatica/experiments/benchmark_cnn_hnp_surface.py --leakage-mode TEMPORAL
```

Results are saved in the selected model directory and include the
`leakage_model` column. A valid comparison between models requires
the same seeds, splits, noise, and CNN/HNP configuration; historical
HW results must not be treated as metrics for the temporal model.

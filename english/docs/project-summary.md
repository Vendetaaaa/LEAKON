# Project Summary

Our project aims to recover partial information about the secret nonce
used when generating ECDSA signatures. By generating noisy artificial
signals, we model, in simplified form, information that might be
observed through a side channel. The partially recovered bits are
converted into useful information for formulating a Hidden Number
Problem (HNP).

The research combines two fields: mathematics and computer science.
Together, they create an experimental chain that begins with Diophantine
approximation theory and proceeds to side-channel-style analysis
simulation, signal classification, and HNP formulation.

## 1. Mathematics

Mathematics underlies the code and algorithms. The geometric problem is
converted into an arithmetic problem through the relation (p = Rq),
where (p) and (q) represent parameters describing the lines and (R) is
the ratio of their slopes. When this ratio is rational, the equation
admits families of integer solutions, corresponding to configurations
in which the lines intersect exactly at a common point.

When the ratio is irrational, exact concurrence does not occur, and
the project analyzes how closely the lines can approach that situation.
The concepts introduced to describe this approach include concurrence
defect, Diophantine approximation, and the rate at which solutions
approach ideal values. The theoretical results are verified through
numerical experiments, which analyze asymptotic constants, transfer of
the Diophantine exponent, the three-distance property, and the relation
between the obtained arithmetic solutions and geometric intersection
positions.

## 2. Computer Science and Cryptography

The cryptographic simulation uses the ECDSA algorithm on the secp256k1
curve. Each experiment uses digital signatures, and every signature has
a corresponding message, hash, `(r)` and `(s)` values, and secret nonce.

The reference configuration considers the nonce's first 12 bits available.
The public attack set and ground-truth files have different roles, but
the distributed archive retains complete nonces in
`informatica/artifacts/oracle_dataset.json`; the explicit private-key field
is redacted. Because complete nonces together with ECDSA signatures permit
derivation of the synthetic key, the oracle file is an evaluation artifact,
not valid input for a blind attack. The CNN → HNP pipeline must use public
data and predicted prefixes, while ground truth is used only after the
solver execution for evaluation.

## 3. Gaussian Noise

After generating the signatures, artificial signals are constructed using
the Hamming Weight model. Gaussian noise is added under controlled
conditions to reproduce, in simplified form, imperfections encountered
in physical measurements.

For the base configuration (`sigma = 0.15`), the 100-epoch reference
run obtained 99.65% bit accuracy and 95.83% prefix accuracy on
validation, and 100.00% and 100.00% on test (40/40).

For the quantitative evaluation, the project ran 20 configurations:
`ell ∈ {8, 10, 12, 14}`, `sigma ∈ {0, 0.05, 0.10, 0.15, 0.20}`, and
`m = 40`. Each configuration aggregates 100 seeds, from `1000` through
`1099`, with 100 training epochs. The results are in
`results/synthetic/HW/tables/cnn-hnp-parameter-sweep-summary.csv`.

## 4. HNP and LLL

In the final stage, nonce prefixes become HNP constraints, and the
problem is formulated as a lattice reduced with LLL. The reference run
for the base configuration is recorded as `PASS`: the solver used
`fpylll`, recovered the synthetic key, and validated it against ground
truth. This result concerns the base instance, not every configuration
in the benchmark.

The 100-seed sweep shows the difference between the oracle and prefixes
predicted by the CNN. For `m = 40`, recovery results are presented below.

**Note about sweep timing in the archive:** the raw results were generated
by an earlier version of the orchestrator, which checked compatibility
with the synthetic key before LLL. The current code runs the solver before
checking against ground truth. In historical files, timings for skipped
instances are conditional and must not be presented as end-to-end timings.
For the current end-to-end comparison, see the CNN ensemble + robust HNP
experiment in the next section.

| Leaked bits `ell` | HNP Oracle | CNN → HNP for `sigma = 0, 0.05, 0.10, 0.15, 0.20` |
| ---: | ---: | --- |
| 8 | `0/100` in all configurations | `0/100`, `0/100`, `0/100`, `0/100`, `0/100` |
| 10 | `100/100` in all configurations | `61/100`, `51/100`, `47/100`, `6/100`, `0/100` |
| 12 | `100/100` in all configurations | `69/100`, `44/100`, `33/100`, `4/100`, `0/100` |
| 14 | `100/100` in all configurations | `70/100`, `49/100`, `40/100`, `3/100`, `0/100` |

CNN bit and prefix accuracy decreases as noise increases. High bit
accuracy does not guarantee a completely correct prefix, and prefix
errors affect HNP recovery. Values in the sweep remain experimental
results for the configurations and seeds tested. The
`cnn-hnp-baseline.csv` table retains baseline results, and
`hnp-oracle-thresholds.csv` contains oracle results for 400 combinations.

The reference solver execution uses the `fpylll` backend; reported
artifacts are in `informatica/artifacts/pipeline_results.json` and
`informatica/artifacts/hnp_result.json`. The private key is redacted in
the public artifacts.

## 5. Comparative Evaluation: Individual CNN, Ensemble, and Robust HNP

To evaluate separately the contribution of CNN model combination and
robust HNP recovery, a comparative benchmark was performed over
100 distinct seeds (`1200`–`1299`).

The experiment configuration was `ell = 12` nonce-prefix bits,
`sigma = 0.10`, `m = 40` signatures for the attack, 160 samples
generated for each seed, and 100 training epochs. Five CNN models
with different initializations were trained for the ensemble on the
same data split. Their predictions were combined by averaging the
classification probabilities. The ensemble's first model was also used
as the individual CNN baseline.

| Method | Recovered keys | Recovery rate |
| --- | ---: | ---: |
| Individual CNN + standard HNP | 43/100 | 43% |
| CNN ensemble + standard HNP | 54/100 | 54% |
| CNN ensemble + robust HNP | 90/100 | 90% |
| Oracle + HNP | 100/100 | 100% |

Mean bit accuracy was 99.758% for the individual CNN and 99.871% for
the ensemble. Mean complete-prefix accuracy increased from 97.125%
to 98.450%.

In the tested configuration, the ensemble increased the key-recovery
rate by 11 percentage points compared with the individual CNN.
Applying robust HNP recovery added another 36 percentage points over
CNN ensemble + standard HNP.

### 5.1. Robust HNP Recovery

The robust method first attempts key recovery using all 40 signatures.
If no valid key is identified, the algorithm successively tries subsets
obtained by removing a single signature.

Each recovered candidate is verified by comparison with the public ECDSA
key. The oracle private key is not used to choose subsets or accept
candidates in this procedure.

In the 100-seed benchmark, the method recovered the key in all 54
experiments with 40/40 correct prefixes and in all 36 experiments
with 39/40 correct prefixes. The 10 failures corresponded to experiments
with at least two incorrect prefixes.

The algorithm performed an average of 12.11 LLL attempts, with a
maximum of 41 attempts. The mean time for the robust HNP stage was
approximately 0.656 seconds per experiment, excluding CNN training.

### 5.2. Reproducibility

The benchmark used the `fpylll` backend. Results were reproduced in
a second run, with no differences across the 100 seeds in the compared
fields concerning correct prefixes, key recovery, and robust HNP attempts.

The experimental files are available in:

- `results/synthetic/HW/tables/comparison-100-seeds.csv`
- `results/synthetic/HW/tables/comparison-100-seeds-summary.csv`

## Additional Validation: Temporal Model and HNP Sensitivity

The values below are retained separately in the [results index](../results/README.md). The archive does not include the raw CSVs or original scripts for reproducing these values; they must be treated as a documented summary, not as a fully reproducible benchmark.

The synthetic temporal model uses the relation:
L_i = 0.2*b_i + 0.6*b_(i-1) + 0.2*b_(i-2) + epsilon_i,
with Gaussian noise of standard deviation 0.15.

The benchmark uses 160 ECDSA examples, divided into
96 train, 24 validation, and 40 test. The 100 seeds
(1400–1499) change the noise and CNN initialization, not the private
key or dataset split.

### Method Comparison

| Method | Bit accuracy | Exact prefixes |
|---|---:|---:|
| Threshold | 54.41% | 0.00% |
| Viterbi | 93.95% | 56.55% |
| CNN raw | 85.66% | 12.85% |
| CNN diff | 69.49% | 2.02% |
| CNN both | 88.86% | 24.95% |

Viterbi uses the known coefficients of the temporal model,
representing an informed baseline for the channel.

### HNP Sensitivity to Errors

In the experiment with 40 signatures and 12 MSB bits:
- 0 incorrect prefixes: standard 10/10, robust 10/10;
- 1 incorrect prefix: standard 0/10, robust 10/10;
- 2 or more incorrect prefixes: both methods 0/10
  in the configurations tested.

### CNN-Confidence Selection

For 20 signatures, confidence-based selection obtained
35.30% exact prefixes, compared with 23.80% for random
selection. The improvement was insufficient for key
recovery in the evaluated temporal configuration.

These results are distinct from the ensemble CNN + robust HNP
experiment on the cumulative model, where key recovery succeeded
in 90/100 cases.

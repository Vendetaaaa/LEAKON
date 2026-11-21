Arhitectura proiectului ( exceptând .git ):

```text
RoSEF26-27/
|-- .github/
|   |-- workflows/
|   |   |-- benchmark.yml
|   |   |-- codeql.yml
|   |   |-- pages.yml
|   |   |-- reproducibility.yml
|   |   `-- tests.yml
|   `-- FUNDING.yml
|-- ARCHITECTURE.md
|-- docs/
|   |-- ethics.md
|   |-- experimental-protocol.md
|   |-- further-notes.md
|   |-- methodology.md
|   |-- project-guide.md
|   |-- project-motivation.md
|   |-- project-overview.md
|   |-- project-summary.md
|   |-- reproducibility.md
|   |-- research-question.md
|   `-- some-results.md
|-- english/
|   |-- docs/
|   |   |-- ethics.md
|   |   |-- experimental-protocol.md
|   |   |-- further-notes.md
|   |   |-- methodology.md
|   |   |-- project-guide.md
|   |   |-- project-motivation.md
|   |   |-- project-overview.md
|   |   |-- project-summary.md
|   |   |-- reproducibility.md
|   |   |-- research-question.md
|   |   `-- some-results.md
|   |-- notebooks/
|   |   |-- 01_data-generation.ipynb
|   |   |-- 02_baseline.ipynb
|   |   |-- 03_cnn-training.ipynb
|   |   |-- 04_final-results.ipynb
|   |   |-- 05_trace-validation.ipynb
|   |   `-- README.md
|   |-- papers/
|   |   |-- arithmetic_concurrence.pdf
|   |   `-- Recovery_of_private_keys.pdf
|   |-- results/
|   |   `-- README.md
|   `-- README.md
|-- informatica/
|   |-- artifacts/
|   |   |-- bridge_results.json
|   |   |-- cnn_predictions.json
|   |   |-- dataset_metadata.json
|   |   |-- dataset_split.json
|   |   |-- diophantine_bridge_verification.png
|   |   |-- diophantine_results.json
|   |   |-- hnp_result.json
|   |   |-- oracle_dataset.json
|   |   |-- pipeline_results.json
|   |   |-- profile_dataset.json
|   |   |-- public_dataset.json
|   |   |-- README.txt
|   |   `-- trace_validation_metadata.json
|   |-- cnn/
|   |   |-- config.yaml
|   |   |-- evaluate.py
|   |   |-- export_weights.py
|   |   |-- model.py
|   |   |-- model_weights.pth
|   |   |-- train.py
|   |   `-- utils.py
|   |-- dataset/
|   |   |-- sample/
|   |   |   |-- dataset_metadata.json
|   |   |   |-- dataset_split.json
|   |   |   |-- profile_dataset.json
|   |   |   `-- public_dataset.json
|   |   |-- capture_esp32.py
|   |   |-- generate.py
|   |   `-- README.md
|   |-- ECDSA/
|   |   |-- cnn_nonce_analysis5.py
|   |   |-- dataset_generator3.py
|   |   |-- diophantine_experiments7.py
|   |   |-- ecdsa_leakage_model4.py
|   |   |-- ecdsa_simulator2.py
|   |   |-- hnp_attack1.py
|   |   |-- hnp_utils.py
|   |   |-- independent_verification.py
|   |   |-- Lattice_key6.py
|   |   |-- main.py
|   |   `-- theory_to_experiment_bridge8.py
|   |-- esp32/
|   |   `-- firmware/
|   |       `-- main/
|   |           |-- docs/
|   |           |   `-- COMMAND_MENU_COMPLETE_RO.md
|   |           |-- src/
|   |           |   |-- batch/
|   |           |   |   |-- batch.cpp
|   |           |   |   `-- batch.h
|   |           |   |-- benchmark/
|   |           |   |   |-- benchmark.cpp
|   |           |   |   `-- benchmark.h
|   |           |   |-- button/
|   |           |   |   |-- button.cpp
|   |           |   |   `-- button.h
|   |           |   |-- core/
|   |           |   |   `-- config.h
|   |           |   |-- crypto/
|   |           |   |   |-- ecdsa_exp.cpp
|   |           |   |   `-- ecdsa_exp.h
|   |           |   |-- experiment/
|   |           |   |   |-- experiment.cpp
|   |           |   |   `-- experiment.h
|   |           |   |-- ina219/
|   |           |   |   |-- ina219.cpp
|   |           |   |   `-- ina219.h
|   |           |   |-- oled/
|   |           |   |   |-- leakon_logo.h
|   |           |   |   |-- oled.cpp
|   |           |   |   `-- oled.h
|   |           |   |-- protocol/
|   |           |   |   |-- protocol.cpp
|   |           |   |   `-- protocol.h
|   |           |   |-- rgbled/
|   |           |   |   |-- rgbled.cpp
|   |           |   |   `-- rgbled.h
|   |           |   |-- selftest/
|   |           |   |   |-- selftest.cpp
|   |           |   |   `-- selftest.h
|   |           |   `-- test_vectors/
|   |           |       |-- test_vectors.cpp
|   |           |       `-- test_vectors.h
|   |           |-- tests/
|   |           |   |-- test_hardware_command_contract.py
|   |           |   `-- test_rgb_command_status.py
|   |           |-- libraries.txt
|   |           |-- main.ino
|   |           `-- Scheme.png
|   |-- experiments/
|   |   |-- benchmark_cnn_hnp_ensemble.py
|   |   |-- benchmark_cnn_hnp_surface.py
|   |   |-- benchmark_hnp_thresholds.py
|   |   `-- run_experiments.py
|   |-- lattice/
|   |   |-- __init__.py
|   |   |-- README.md
|   |   `-- solver.py
|   |-- simulator/
|   |   |-- cli.py
|   |   |-- config.py
|   |   |-- generator.py
|   |   `-- noise.py
|   |-- __init__.py
|   `-- README.md
|-- matematica/
|   |-- experiments/
|   |   |-- concurenta/
|   |   |   |-- figs_con/
|   |   |   |   |-- 01_concurenta_exacta.png
|   |   |   |   |-- 02_near_concurenta_sqrt2.png
|   |   |   |   `-- 03_concurenta_aproximativa_k4.png
|   |   |   |-- 01_concurenta_exacta.py
|   |   |   |-- 02_near-concurenta.py
|   |   |   `-- 03_cocurenta-aprox.py
|   |   `-- graficele_matematica/
|   |       |-- figs_cert/
|   |       |   |-- 01_plan_Vk_retea_rezonanta.png
|   |       |   |-- 02_deltaM_sqrt2.png
|   |       |   |-- 03_deltaM_phi.png
|   |       |   |-- 04_tranzitie_rationala_zero_defect.png
|   |       |   `-- 05_defect_simultan_k4.png
|   |       |-- 01_plan_Vk_retea_rezonanta.py
|   |       |-- 02_deltaM_sqrt2.py
|   |       |-- 03_deltaM_phi.py
|   |       |-- 04_tranzitie_rationala_zero_defect.py
|   |       `-- 05_defect_simultan_k4.py
|   |-- k4-k5-verification.md
|   `-- README.md
|-- misc/
|   |-- Echipa.png
|   |-- Github1.png
|   |-- Github2.png
|   |-- LEAKON.png
|   |-- QR-github.svg
|   |-- QR-leakon.svg
|   |-- Team2.jpg
|   `-- Team3.jpg
|-- notebooks/
|   |-- 01_data-generation.ipynb
|   |-- 02_baseline.ipynb
|   |-- 03_cnn-training.ipynb
|   |-- 04_final-results.ipynb
|   |-- 05_trace-validation.ipynb
|   `-- README.md
|-- papers/
|   |-- source/
|   |   |-- arithmetic_concurrence.tex
|   |   |-- README.md
|   |   `-- Recovery_of_private_keys.tex
|   |-- arithmetic_concurrence.pdf
|   `-- Recovery_of_private_keys.pdf
|-- results/
|   |-- esp32_results/
|   |   |-- ecdsa_sign_verify_samples.csv
|   |   |-- INA219.md
|   |   |-- ina219_samples.csv
|   |   |-- PulseView1.png
|   |   |-- PulseView2.png
|   |   |-- rnts_ecdsa_operation_metadata.csv
|   |   |-- rnts_fullimp_operation_summary.csv
|   |   `-- rnts_ina219_measurements.csv
|   |-- figures/
|   |   |-- diophantine_experiments.png
|   |   |-- trace-validation-example.png
|   |   |-- trace-validation-shift-correlation.png
|   |   `-- trace-validation-snr.png
|   |-- raw/
|   |   `-- trace-validation-100.npz
|   |-- tables/
|   |   |-- cnn-hnp-baseline.csv
|   |   |-- cnn-hnp-parameter-sweep-summary.csv
|   |   |-- cnn-hnp-parameter-sweep.csv
|   |   |-- comparison-100-seeds-summary.csv
|   |   |-- comparison-100-seeds.csv
|   |   |-- final-results.csv
|   |   |-- hnp-oracle-thresholds.csv
|   |   |-- trace-validation-100.csv
|   |   `-- trials-with-cnn.csv
|   `-- README.md
|-- tests/
|   |-- test_benchmark_hnp_surface.py
|   |-- test_ecdsa.py
|   |-- test_hnp.py
|   |-- test_hnp_regressions.py
|   |-- test_independent_verification.py
|   |-- test_math.py
|   |-- test_model.py
|   |-- test_reference_artifacts.py
|   |-- test_simulator.py
|   `-- test_smoke.py
|-- tools/
|   `-- rosef/
|       |-- run_rosef.py
|       |-- run_rosef_batch.bat
|       |-- run_rosef_batch.py
|       `-- test_temporal_augmentation.py
|-- website/
|   |-- paper/
|   |   `-- Recovery_of_private_keys.pdf
|   |-- source/
|   |   |-- informatica/
|   |   |   |-- capture_esp32.py
|   |   |   |-- cnn_config.yaml
|   |   |   |-- cnn_model.py
|   |   |   |-- cnn_utils.py
|   |   |   |-- dataset_generator3.py
|   |   |   |-- ecdsa_leakage_model4.py
|   |   |   |-- hnp_attack1.py
|   |   |   |-- independent_verification.py
|   |   |   `-- Lattice_key6.py
|   |   |-- dataset_metadata.json
|   |   |-- docs_experimental-protocol.md
|   |   |-- docs_methodology.md
|   |   |-- docs_project-overview.md
|   |   |-- docs_project-summary.md
|   |   |-- docs_reproducibility.md
|   |   |-- docs_research-question.md
|   |   |-- final-results.csv
|   |   |-- hnp_result.json
|   |   |-- papers_Recovery_of_private_keys.tex
|   |   |-- pipeline_results.json
|   |   `-- README.md
|   |-- app.js
|   |-- data.js
|   |-- index.html
|   `-- styles.css
|-- .gitignore
|-- CITATION.cff
|-- CONTRIBUTING.md
|-- LICENSE
|-- Q&A.txt
|-- README.md
|-- requirements.txt
`-- SECURITY.md
```

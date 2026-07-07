# MalTrust-Hybrid-GCT

**MalTrust-Hybrid-GCT** is a leakage-aware, explainable, externally validated, and reproducibility-oriented hybrid framework for static Windows Portable Executable (PE) malware detection.

The framework combines a strict-clean 699-dimensional static PE feature representation, a LightGBM source detector, a group-aware GCT-v3 auxiliary detector, validation-derived GCT-v3 temperature calibration, and a validation-trained logistic meta-calibrator over ten probability, confidence, and disagreement features.

> **Primary scope:** static Windows PE malware detection research.  
> **Primary contribution:** validation-locked hybrid framework design, leakage-aware evaluation, external unique-SHA validation, bounded robustness analysis, SHAP-based source-detector explainability, and reproducibility/artifact-integrity support.

---

## Repository description

Leakage-aware, explainable, externally validated Hybrid-GCT framework for static Windows PE malware detection using strict-clean EMBER2024 features, LightGBM, GCT-v3, validation-locked meta-calibration, SHAP, BODMAS replication, and reproducibility artifacts.

Suggested GitHub topics:

```text
malware-detection, windows-pe, static-analysis, ember2024, bodmas, lightgbm, gct, explainable-ai, shap, cybersecurity, reproducibility, machine-learning
```

---

## Key features

- Strict-clean static PE feature construction with leakage-prone metadata removal.
- SHA-256 de-duplication and SHA-disjoint train/validation/test splitting.
- LightGBM source detector for structured PE feature learning.
- Group-aware GCT-v3 auxiliary detector.
- Validation-derived temperature scaling for GCT-v3.
- Validation-trained logistic meta-calibrator.
- Ten-dimensional probability, confidence, and disagreement meta-feature vector.
- Validation-locked threshold selection using MCC.
- Internal SHA-disjoint held-out evaluation.
- External unique-SHA EMBER2024 Win64 validation without retraining.
- Paired McNemar statistical testing against source detectors.
- Calibration analysis using Brier score, ECE, and log loss.
- Temporal external validation.
- Bounded synthetic robustness stress testing.
- BODMAS independent static PE learning-protocol replication.
- SHAP-based explainability for the LightGBM source detector.
- Hash-chained artifact integrity auditing for reproducibility and traceability.

---

## Final framework summary

| Component | Final configuration |
|---|---|
| Framework | MalTrust-Hybrid-GCT |
| Task | Binary benign/malware PE classification |
| Primary dataset | EMBER2024 Win64 |
| Feature representation | 699 strict-clean static PE features |
| Source detector | LightGBM |
| Auxiliary detector | GCT-v3 |
| Auxiliary calibration | Temperature scaling on validation data |
| Hybrid fusion | Logistic meta-calibrator |
| Meta-features | 10 probability, confidence, and disagreement features |
| Threshold | Validation-selected threshold, `tau_H = 0.48` |
| Internal test | 75,000 SHA-disjoint samples |
| External test | 119,993 unique-SHA EMBER2024 Win64 samples |
| Explainability | SHAP attribution for LightGBM source detector |
| Reproducibility | Configuration files, manifests, reports, and hash-chained artifact records |

---

## Dataset policy

This repository does **not** redistribute raw malware binaries, raw EMBER2024 dataset contents, or raw BODMAS dataset contents.

The experiments use third-party malware datasets subject to their own licensing and access conditions. Users must obtain those datasets from their official sources and comply with their respective terms.

This repository may provide:

- preprocessing scripts,
- feature-schema definitions,
- split-generation logic,
- configuration files,
- evaluation scripts,
- trained-model metadata,
- generated metric reports,
- figure-generation scripts,
- reproducibility notes,
- hash/integrity manifests,
- notebook-based development artifacts.

This repository must not be used to distribute malware samples or unauthorized dataset copies.

---

## Method overview

The framework follows a validation-locked pipeline:

1. Acquire EMBER2024 Win64 PE feature data according to dataset access rules.
2. Audit raw and nested fields for leakage-prone attributes.
3. Remove identity-based, temporal, antivirus-derived, behavioral, family-level, and non-static fields.
4. Construct the strict-clean 699-dimensional static PE feature representation.
5. Apply SHA-256 de-duplication.
6. Construct SHA-disjoint train/validation/test partitions.
7. Train LightGBM on the training partition.
8. Train GCT-v3 on grouped strict-clean PE feature groups.
9. Temperature-calibrate GCT-v3 on validation data.
10. Generate frozen LightGBM and GCT-v3 validation probabilities.
11. Construct a ten-dimensional hybrid meta-feature vector.
12. Train logistic meta-calibrator using validation predictions only.
13. Select the final decision threshold using validation MCC.
14. Freeze source models, temperature parameter, meta-calibrator, and threshold.
15. Evaluate on internal held-out and external unique-SHA sets without retraining or re-thresholding.
16. Perform paired statistical testing, temporal validation, calibration analysis, SHAP analysis, BODMAS replication, and artifact integrity auditing.

---

## Hybrid meta-feature vector

For each sample, let `p_L` be the LightGBM malware probability and `p_G` be the calibrated GCT-v3 malware probability.

The final hybrid meta-calibrator uses:

```text
[p_L,
 p_G,
 |p_L - p_G|,
 (p_L + p_G) / 2,
 max(p_L, p_G),
 min(p_L, p_G),
 |p_L - 0.5|,
 |p_G - 0.5|,
 Delta_c,
 p_L * p_G]
```

where:

```text
Delta_c = ||p_L - 0.5| - |p_G - 0.5||
```

The final hybrid probability is produced by a logistic meta-calibrator trained only on validation predictions.

---

## Reported results

### Internal SHA-disjoint held-out test

| Model | Accuracy | F1 | MCC | ROC-AUC | PR-AUC | Brier | ECE |
|---|---:|---:|---:|---:|---:|---:|---:|
| MalTrust-Hybrid-GCT | 0.983373 | 0.983349 | 0.966751 | 0.998445 | 0.998581 | 0.013244 | 0.005443 |
| Final LightGBM | 0.980907 | 0.980859 | 0.961825 | 0.998052 | 0.998211 | 0.014934 | 0.009497 |
| MalTrust-GCT-v3 | 0.979960 | 0.979921 | 0.959927 | 0.995740 | 0.996248 | 0.016814 | 0.005699 |

### External unique-SHA EMBER2024 Win64 validation

| Model | Accuracy | F1 | MCC | ROC-AUC | PR-AUC | Brier | ECE | Log loss |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| MalTrust-Hybrid-GCT | 0.977500 | 0.977509 | 0.954998 | 0.997254 | 0.997500 | 0.018354 | 0.010133 | 0.073535 |
| Final LightGBM | 0.975015 | 0.974987 | 0.950033 | 0.996913 | 0.997164 | 0.019248 | 0.005880 | 0.069523 |
| MalTrust-GCT-v3 | 0.970405 | 0.970320 | 0.940830 | 0.991921 | 0.993106 | 0.024598 | 0.008265 | 0.102218 |

Calibration interpretation is intentionally bounded: the hybrid framework improves external Brier score, but it does not dominate all external calibration metrics because Final LightGBM has lower external ECE and log loss.

---

## BODMAS replication boundary

BODMAS is used as an independent static PE learning-protocol replication benchmark. It is **not** direct frozen-model transfer validation of the EMBER2024-trained MalTrust-Hybrid-GCT framework because BODMAS uses a different feature representation.

| Evaluation | Samples | Features | Accuracy | F1 | MCC | ROC-AUC |
|---|---:|---:|---:|---:|---:|---:|
| Held-out LightGBM replication | 20,166 | 2,381 | 0.997471 | 0.997030 | 0.994850 | 0.999884 |
| 5-fold out-of-fold replication | 134,435 | 2,381 | 0.997307 | 0.996838 | 0.994494 | 0.999860 |

---

## Robustness boundary

Synthetic robustness stress testing is interpreted only as controlled feature-space stress analysis.

The repository and manuscript do **not** claim:

- proof of adversarial robustness,
- evaluation on fully functional packed malware unless such samples are actually generated and tested,
- robustness against all obfuscated, polymorphic, or adversarial PE binaries,
- universal superiority across all malware families or future threat distributions.

Stronger executable-level validation remains future work.

---

## Deployment boundary

MalTrust-Hybrid-GCT is architecturally compatible with static PE malware-screening workflows. The final fusion layer is compact because it uses a ten-dimensional probability-level meta-feature vector after source-model scoring.

The repository does **not** claim production-ready deployment performance unless separate benchmark reports are provided.

The following remain outside the present experimental scope:

- production latency benchmarking,
- throughput benchmarking,
- endpoint-scale memory profiling,
- concurrent-request handling,
- streaming malware analysis,
- analyst-centered usability evaluation,
- production SOC integration.

---

## Reproducibility

The complete LightGBM, GCT-v3, meta-calibrator, random-seed, preprocessing, feature-schema, and evaluation configurations should be placed under the repository configuration directory.

Recommended reproducibility artifacts:

```text
configs/
  lightgbm_config.json
  gct_v3_config.json
  meta_calibrator_config.json
  preprocessing_config.json
  random_seeds.json
  feature_schema_699.json
  evaluation_config.json

manifests/
  dataset_manifest.json
  split_manifest.json
  artifact_hash_chain.json

reports/
  internal_test_metrics.json
  external_test_metrics.json
  temporal_metrics.json
  calibration_metrics.json
  bodmas_replication_metrics.json
  mcnemar_results.json
```

---

## Suggested repository structure

```text
MalTrust-Hybrid-GCT/
  README.md
  LICENSE
  MODEL_DEVELOPMENT_LICENSE_AND_CODE_NOTE.md
  DATASET_AND_REPRODUCIBILITY_NOTE.md
  SECURITY_AND_ETHICAL_USE.md

  notebooks/
    framework-development-24-06-2026.ipynb

  src/
    data/
      build_dataset.py
      deduplicate_sha.py
      split_sha_disjoint.py
    features/
      extract_pe_features.py
      strict_clean_filter.py
      feature_schema.py
    models/
      train_lightgbm.py
      train_gct_v3.py
      calibrate_gct.py
      train_meta_calibrator.py
      predict_hybrid.py
    evaluation/
      evaluate_internal.py
      evaluate_external.py
      mcnemar_test.py
      calibration_metrics.py
      temporal_analysis.py
      robustness_stress_test.py
      bodmas_replication.py
    explainability/
      shap_lightgbm.py
    integrity/
      hash_chain.py

  configs/
    lightgbm_config.json
    gct_v3_config.json
    meta_calibrator_config.json
    preprocessing_config.json
    random_seeds.json
    evaluation_config.json

  reports/
    .gitkeep

  figures/
    .gitkeep
```

---

## Installation

Create a Python environment and install the required packages.

```bash
python -m venv .venv
source .venv/bin/activate   # Linux/macOS
# .venv\Scripts\activate    # Windows

pip install -U pip
pip install numpy pandas scikit-learn lightgbm torch shap matplotlib joblib tqdm psutil
```

Exact versions used in final experiments should be recorded in `requirements.txt` or `environment.yml`.

---

## Usage

The repository should be run only after the user has obtained the required datasets under their own valid access permissions.

Example workflow:

```bash
python src/data/build_dataset.py --config configs/preprocessing_config.json
python src/data/deduplicate_sha.py --config configs/preprocessing_config.json
python src/data/split_sha_disjoint.py --config configs/preprocessing_config.json

python src/models/train_lightgbm.py --config configs/lightgbm_config.json
python src/models/train_gct_v3.py --config configs/gct_v3_config.json
python src/models/calibrate_gct.py --config configs/gct_v3_config.json
python src/models/train_meta_calibrator.py --config configs/meta_calibrator_config.json

python src/evaluation/evaluate_internal.py --config configs/evaluation_config.json
python src/evaluation/evaluate_external.py --config configs/evaluation_config.json
python src/evaluation/mcnemar_test.py --config configs/evaluation_config.json
python src/explainability/shap_lightgbm.py --config configs/evaluation_config.json
python src/integrity/hash_chain.py --config configs/evaluation_config.json
```

---

## Legacy naming note

Some early development notebooks or intermediate artifacts may contain the internal development label **MalTrust-XAI**. The final framework name used for the manuscript and repository is:

```text
MalTrust-Hybrid-GCT
```

The final manuscript claims, results, and repository documentation should use **MalTrust-Hybrid-GCT** consistently.

---

## Citation

If you use this repository, cite the associated manuscript:

```bibtex
@misc{singh2026maltrusthybridgct,
  title        = {MalTrust-Hybrid-GCT: A Leakage-Aware, Explainable, and Externally Validated Hybrid Framework for Windows PE Malware Detection},
  author       = {Bhagwant Singh},
  year         = {2026},
  note         = {Preprint / manuscript under submission}
}
```

Update this citation after journal acceptance with the final DOI and publication details.

---

## License

Recommended license structure:

- **Code:** MIT License.
- **Documentation and figures:** Creative Commons Attribution 4.0 International, if desired.
- **Datasets:** governed by the original EMBER2024 and BODMAS dataset licenses/access terms.
- **Trained models:** release only if permitted by dataset terms, institutional rules, and cybersecurity-safety requirements.

See `MODEL_DEVELOPMENT_LICENSE_AND_CODE_NOTE.md` for detailed usage boundaries.

---

## Ethical and security use

This repository is intended for defensive cybersecurity research, malware-detection validation, reproducibility, and academic evaluation.

Do not use this repository to:

- distribute malware,
- evade malware detectors,
- build or optimize malicious software,
- bypass security products,
- deploy unvalidated automated blocking in high-risk environments,
- violate dataset licenses or access restrictions.


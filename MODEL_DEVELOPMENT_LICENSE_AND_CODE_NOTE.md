# Model Development, License, and Code Note

## Project name

**MalTrust-Hybrid-GCT**

Full title:

**MalTrust-Hybrid-GCT: A Leakage-Aware, Explainable, and Externally Validated Hybrid Framework for Windows PE Malware Detection**

---

## Purpose of the repository

This repository provides the code, configuration structure, reproducibility notes, and supporting artifacts for the development and evaluation of MalTrust-Hybrid-GCT.

The project is intended for:

- academic malware-detection research,
- static Windows PE malware-detection validation,
- leakage-aware machine-learning experimentation,
- reproducibility-focused cybersecurity evaluation,
- explainability and artifact-integrity support.

The repository is **not** intended for malware generation, malware deployment, evasion engineering, or offensive cybersecurity use.

---

## Model-development summary

MalTrust-Hybrid-GCT was developed as a validation-oriented hybrid framework for static Windows PE malware detection.

The final framework contains:

1. strict-clean static PE feature construction,
2. SHA-256 de-duplication,
3. SHA-disjoint train/validation/test splitting,
4. LightGBM source detector,
5. group-aware GCT-v3 auxiliary detector,
6. validation-derived GCT-v3 temperature scaling,
7. ten-dimensional probability-level hybrid meta-feature vector,
8. validation-trained logistic meta-calibrator,
9. validation-locked threshold selection,
10. internal SHA-disjoint evaluation,
11. external unique-SHA EMBER2024 validation,
12. temporal validation,
13. synthetic robustness stress testing with bounded claims,
14. BODMAS independent static PE learning-protocol replication,
15. SHAP-based LightGBM source-detector explainability,
16. hash-chained artifact integrity auditing.

---

## Development phases

### Phase 1: Dataset discovery and reproducibility initialization

The dataset files were discovered, selected, fingerprinted, and recorded using reproducibility configuration files. Dataset manifests and file hashes were used to support traceability.

### Phase 2: Leakage audit

Raw and nested fields were audited for leakage risk. Identity-based, temporal, antivirus-derived, behavioral, family-level, post-analysis, and non-static attributes were excluded.

Representative removed attributes include:

- `md5`
- `sha1`
- `sha256`
- `tlsh`
- `first_submission_date`
- `last_analysis_date`
- `week_id`
- `detection_ratio`
- `family`
- `family_confidence`
- `behavior`
- `header.coff.timestamp`
- `authenticode.latest_signing_time`
- `authenticode.signing_time_diff`

### Phase 3: Strict-clean feature construction

A final strict-clean 699-dimensional static PE feature representation was constructed for EMBER2024 Win64 data.

The retained evidence includes deployable static PE characteristics such as:

- byte histograms,
- byte-entropy features,
- string statistics,
- general PE properties,
- header attributes,
- section-level properties,
- imports,
- exports,
- data directories,
- rich-header attributes,
- authenticode-derived structural indicators after leakage filtering.

### Phase 4: SHA-256 de-duplication and splitting

Duplicate SHA-256 entries were removed before final evaluation. The internal dataset was split into:

- 350,000 training samples,
- 75,000 validation samples,
- 75,000 internal held-out test samples.

The split was SHA-disjoint to reduce duplicate-file inflation and memorization risk.

### Phase 5: Source-model training

A LightGBM source detector was trained on the strict-clean 699-dimensional static PE representation. The trained source detector was frozen before held-out and external evaluation.

### Phase 6: GCT-v3 auxiliary training

A group-aware GCT-v3 auxiliary detector was trained over semantically grouped PE feature subsets. Its output was temperature-calibrated using validation data.

### Phase 7: Hybrid meta-calibration

Frozen LightGBM and calibrated GCT-v3 validation probabilities were used to construct a ten-dimensional meta-feature vector. A logistic meta-calibrator was trained only on validation predictions.

### Phase 8: Validation-locked thresholding

The final decision threshold was selected using validation MCC and fixed at:

```text
tau_H = 0.48
```

The threshold was frozen before internal held-out and external unique-SHA evaluation.

### Phase 9: Final evaluation

The frozen framework was evaluated on:

- internal SHA-disjoint held-out test set,
- external unique-SHA EMBER2024 Win64 test-period set,
- chronological external periods,
- synthetic robustness stress settings,
- BODMAS independent replication benchmark.

---

## Reproducibility requirements

For full reproducibility, the repository should include or document:

- data-preprocessing code,
- feature-schema files,
- leakage-exclusion lists,
- SHA-disjoint split-generation logic,
- split manifests,
- LightGBM configuration,
- GCT-v3 configuration,
- meta-calibrator configuration,
- random seeds,
- evaluation scripts,
- prediction-output schemas,
- metric-report schemas,
- calibration-analysis scripts,
- McNemar testing scripts,
- SHAP analysis scripts,
- BODMAS replication scripts,
- hash-chain artifact auditing scripts.

Raw malware datasets are not redistributed by this repository.

---

## Dataset licensing and access restrictions

The project uses third-party datasets, including EMBER2024 and BODMAS. These datasets remain subject to their own licensing and access conditions.

This repository does not grant permission to use, copy, redistribute, or publish those datasets.

Users must:

1. obtain datasets from the official source,
2. follow each dataset's license/access terms,
3. avoid redistributing raw malware binaries,
4. avoid uploading restricted dataset contents into this repository,
5. preserve dataset citations and usage restrictions.

---

## Code license

Recommended code license:

```text
MIT License
```

The MIT License allows reuse, modification, and distribution of the code with attribution and license preservation.

The MIT License applies only to code authored for this repository. It does not override dataset licenses, third-party package licenses, institutional policies, or cybersecurity safety restrictions.

---

## Documentation license

Recommended documentation license:

```text
Creative Commons Attribution 4.0 International (CC BY 4.0)
```

This may be used for README content, documentation, non-sensitive figures, and explanatory notes.

However, do not apply CC BY 4.0 to raw datasets, malware samples, restricted artifacts, or third-party content unless permission exists.

---

## Model and artifact release note

Trained models and generated artifacts should be released only when permitted by:

- dataset licenses,
- institutional rules,
- journal policy,
- cybersecurity safety requirements,
- applicable law.

If trained models are released, they should be accompanied by:

- intended-use statement,
- limitations,
- dataset provenance summary,
- hash/integrity manifest,
- configuration files,
- evaluation reports,
- non-guarantee disclaimer.

---

## Security and ethical-use boundary

This repository is intended for defensive and academic use only.

Permitted uses:

- malware-detection research,
- reproducibility testing,
- static PE feature-analysis research,
- validation protocol development,
- explainable AI evaluation,
- academic benchmarking,
- cybersecurity education under safe conditions.

Prohibited or unsupported uses:

- malware generation,
- malware distribution,
- evasion-tool development,
- bypassing antivirus or endpoint detection systems,
- offensive deployment,
- unauthorized analysis of third-party systems,
- redistribution of restricted malware datasets,
- high-impact automated blocking without operational validation.

---

## Development-name consistency note

Some early notebook cells and intermediate files may contain the development label:

```text
MalTrust-XAI
```

The final framework name is:

```text
MalTrust-Hybrid-GCT
```

For manuscript, GitHub, citation, release, and documentation purposes, use **MalTrust-Hybrid-GCT** consistently.

---

## Disclaimer

This repository and associated models are provided for research use. No warranty is provided regarding correctness, security, operational suitability, robustness, or fitness for production deployment.

The authors do not claim:

- universal malware-detection superiority,
- adversarial robustness proof,
- production-ready endpoint performance,
- guaranteed detection of all malware families,
- safe deployment without operational validation.

Use in production security environments requires independent testing, legal approval, operational monitoring, and expert oversight.

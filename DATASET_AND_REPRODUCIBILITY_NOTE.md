# Dataset and Reproducibility Note

## Dataset availability

This repository does not redistribute raw EMBER2024 or BODMAS dataset contents.

Users must obtain the datasets independently from their official sources and comply with the relevant access terms, license conditions, and institutional rules.

## Reproducibility artifacts

The repository may include:

- preprocessing scripts,
- feature-schema definitions,
- leakage-exclusion manifests,
- split-generation scripts,
- split manifests without raw malware content,
- configuration files,
- evaluation scripts,
- metric reports,
- model metadata,
- figures generated from allowed outputs,
- artifact hash-chain records.

## Raw-data restriction

Do not commit:

- raw malware binaries,
- raw EMBER2024 contents,
- raw BODMAS contents,
- private dataset mirrors,
- proprietary vendor data,
- credentials,
- dataset access tokens,
- personally identifiable information,
- unauthorized third-party samples.

## Recommended reproducibility package

A release package may include:

```text
configs/
manifests/
reports/
figures/
notebooks/
src/
README.md
LICENSE
MODEL_DEVELOPMENT_LICENSE_AND_CODE_NOTE.md
DATASET_AND_REPRODUCIBILITY_NOTE.md
SECURITY_AND_ETHICAL_USE.md
```

The package should allow another authorized researcher to reproduce the workflow after obtaining the required datasets through valid channels.

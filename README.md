# slsa_builder

SLSA Level 3 public key store and artifact verification wrapper.

## Overview

This repository is the **primary trust root** for the public key used to verify
SLSA Level 3 provenance attestations. Verification is performed with
[Cosign](https://docs.sigstore.dev/cosign/overview/) using the
`verify-blob-attestation` command against the key stored in this repo.

## Repository Layout

```
keys/
  activesalsa.pub     Trusted PEM-encoded public key for SLSA provenance verification
```

## Prerequisites

- **cosign** — install from
  <https://docs.sigstore.dev/cosign/system_config/installation/>

## Verification

To validate that an artifact's DSSE-envelope attestation was signed by the
trusted builder key:

```bash
cosign verify-blob-attestation \
      --insecure-ignore-tlog \
      --key keys/activesalsa.pub \
      --signature "$dsse_name" \
      --type "https://slsa.dev/provenance/v1" \
      "$artifact_name"
```

| Flag | Purpose |
|------|---------|
| `--insecure-ignore-tlog` | Skip transparency-log lookup (key-based trust only) |
| `--key` | Path to the trusted public key (`keys/activesalsa.pub`) |
| `--signature` | Path to the DSSE-envelope file for the artifact |
| `--type` | Expected SLSA provenance predicate type |

A zero exit code means the attestation signature is valid and the provenance
type matches — confirming SLSA Level 3 provenance for the artifact.


# Public Keys

This directory contains trusted public keys for SLSA L3 artifact and attestation verification.

## Layout

```
keys/
├── activesalsa.pub     # One PEM-encoded public key per trusted builder
└── README.md
```

## Adding a Key

Place the PEM-encoded public key file here, named after the builder identity it represents.
The verification script resolves keys from this directory by builder ID.

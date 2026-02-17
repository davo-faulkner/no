# Notary Repository

This repository serves as a cryptographic ledger for content authentication, implementing a 6-layer "Authentication Stack" to defend against forgery and ensure permanent, verifiable proof of existence for digital artifacts.

## Tooling: The `no` (Notarize) Script

The `no` script is the primary automation engine for this repository. It codifies the authentication stack to ensure every notarization is performed with mathematical rigor and consistency.

### Features
* **Network Pre-check**: Verifies connectivity for temporal witnesses (TSR/OTS) before starting resource-intensive processing.
* **Deterministic Archiving**: Uses `zstd` (Level 15, 6-threads) for high-performance, metadata-preserved compression.
* **Integrated Verification**: Automatically verifies GPG signatures and performs a test restoration to a temporary directory before finalizing.
* **Salted Encryption**: Utilizes GPG symmetric encryption, ensuring every notarization event results in a unique hash even for identical source files.

### Usage
Since the script is in the system `$PATH`, it can be invoked from any directory:
```bash
no <target_file_or_folder>

## Hardware Configuration
Note: The -T6 flag in the tar command is optimized for a 6-core processor (Intel i9). If your system has a different number of CPU cores, you must update this value in the script to match your hardware to avoid process bottlenecks or execution errors.

Stack Execution Order
Layer 1: Integrity: Archive and Symmetric Encryption.

Layer 2: Identity: SHA-512 Hashing and PGP Detached Signing (Identity Witness).

Layer 3: Authority: RFC 3161 Timestamp via FreeTSA (Temporal Witness).

Layer 4: Availability: OpenTimestamps (Bitcoin Blockchain Anchor).

Layer 5: Provenance: Git staging with signed commitments.

Layer 6: Permanence: Public archival via the Wayback Machine.

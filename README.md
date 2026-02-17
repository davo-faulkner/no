# no (Notarize) Script

This repository contains the `no` automation script, the primary submission engine used to generate cryptographic artifacts for the [notary](https://github.com/davo-faulkner/notary) ledger.

## Tooling: The `no` (Notarize) Command

The `no` script codifies the logic of the 6-layer "Authentication Stack" to ensure every notarization is performed with mathematical rigor and consistency.

### Features
* **Network Pre-check**: Verifies connectivity for temporal witnesses (TSR/OTS) before starting resource-intensive processing.
* **Deterministic Archiving**: Uses `zstd` (Level 15, 6-threads) for high-performance, metadata-preserved compression.
* **Integrated Verification**: Automatically verifies GPG signatures and performs a test restoration to a temporary directory before finalizing.
* **Salted Encryption**: Utilizes GPG symmetric encryption, ensuring every notarization event results in a unique hash even for identical source files.

### Usage
Since the script is in the system `$PATH`, it can be invoked from any directory:

    no <filename or directory>

### Hardware Configuration
Note: The -T6 flag in the tar command is optimized for a 6-core processor (Intel i9). If your system has a different number of CPU cores, you must update this value in the script to match your hardware to avoid process bottlenecks or execution errors.

### Stack Execution Order
Layer 1: Integrity: Archive, Symmetric Encryption, and SHA-512 Hashing.

Layer 2: Identity: PGP Cleartext Signing (Identity Witness).

Layer 3: Timestamp (Fast): RFC 3161 Timestamp via FreeTSA (Temporal Witness).

Layer 4: Timestamp (Immutable): OpenTimestamps (Bitcoin Blockchain Anchor).

Layer 5: Provenance: Git staging with signed commits.

Layer 6: Permanence: Public archival via the Wayback Machine.

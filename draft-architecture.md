# PQ-SC-USG Protocol Layered Architecture

**Document ID:** PQ-SC-USG-ARCH-001  
**Version:** Draft v0.3 (August 2026)  
**Settlement chain:** TON only  
**Systems Engineering Level:** Concept → Preliminary Design (PDR ready)  
**Approval Status:** IPT Draft – Pending PDR Gate

This document is the current architecture baseline. It is linked from the [README](README.md).

## Layer 7 – Application Layer

- USG issuance (memo / press release / video)
- Metadata package (timestamp, classification, issuer DID)

## Layer 6 – Cryptographic Primitives Layer

- Hash function: SHA-3-256 at the FIPS boundary; Poseidon2 inside the zkVM circuit
- Digital signature (primary): **FIPS 204 ML-DSA**, default **ML-DSA-87** for CNSA 2.0 / NSS; ML-DSA-65 only for unclassified non-NSS pilots
- Digital signature (fallback): **FIPS 205 SLH-DSA** (SHA2 or SHAKE parameter sets) for lattice-independent diversity; not CNSA 2.0 approved
- Key encapsulation: **FIPS 203 ML-KEM**, default **ML-KEM-1024** for NSS attachment confidentiality; usage per NIST SP 800-227
- Key registry: on-chain PQ public-key fingerprint only (32-byte hash)

## Layer 5 – Zero-Knowledge Proving Subsystem

Prover: RISC-V zkVM (RISC Zero or Succinct SP1 today)

- Proof type: zk-STARK (transparent, hash-only security, no trusted setup)
- Recursion: 2-to-1 aggregation for daily batch issuance
- Modular Verifier Interface (MVI): pluggable proof format so the TON verifier contract ABI stays stable when the off-chain zkVM changes

## Layer 4 – Proof Submission & Verification

- Submission: TON external inbound message to the collection / verifier smart contract
- On-chain verification: TVM STARK verifier; only the proof commitment is stored

## Layer 3 – Blockchain Settlement Layer (TON)

- Unique-item smart contracts: TEP-62 collection + item pattern
- Metadata: TEP-64
- Implementation language: Tolk (FunC is legacy)
- On-chain data: proof commitment + PQ key fingerprint + issuance epoch

## Layer 2 – Immutable Storage Layer

- Primary: IPFS (content-addressed)
- Permanent backup: Arweave (permaweb)
- ML-KEM-wrapped attachments may sit beside the public document CID

## Layer 1 – Public Consumption Layer

- Any TON light client, block explorer, or wallet checks the on-chain commitment and retrieves content by CID

---

**Core systems invariants**

- Quantum-resistant provenance (FIPS 203 / 204 / 205 + zk-STARK)
- No raw PQ signatures or ML-KEM keys stored on-chain
- Zero-trust public verification (no trusted third party)
- Recursive batch issuance
- TON-only settlement (no parallel EVM / SVM / zkEVM path)

---

**Next steps**

- [ ] Finalize Interface Control Document (ICD) for the cryptographic payload on TVM cells
- [ ] Complete PDR package (this document + SRD)

# PQ-SC-USG: Post-Quantum Smart Contract Protocol for USG Official Communications

Production-grade, open-source protocol specification and reference implementation for issuing U.S. Government communications (press releases, speeches, troop-deployment notices, and related official records) as immutable, publicly verifiable **smart-contract records** on **TON**, secured against classical and quantum adversaries.

**Target chain:** TON (The Open Network)  
**Settlement model:** TON collection and item smart contracts ([TEP-62](https://github.com/ton-blockchain/TEPs/blob/master/text/0062-nft-standard.md) unique-item interface, [TEP-64](https://github.com/ton-blockchain/TEPs/blob/master/text/0064-token-data-standard.md) metadata)  
**Cryptography:** NIST FIPS 204 ML-DSA (primary signature), FIPS 205 SLH-DSA (hash-based fallback), FIPS 203 ML-KEM (attachment confidentiality)

Layered architecture: [draft-architecture.md](draft-architecture.md)  
Issuance flow diagram: [draft-workflow.md](draft-workflow.md)  
Terminology: [glossary.md](glossary.md)

---

## 1. Purpose

PQ-SC-USG binds an official communication to a TON smart contract whose mint (issuance) is gated by a post-quantum signature check, proven off-chain and verified on-chain. Any public TON client can confirm that:

1. The content hash matches the declared metadata.
2. The signature verifies against a registered U.S. Government post-quantum public-key fingerprint.
3. The on-chain record is immutable after issuance.

Raw ML-DSA / SLH-DSA signatures and ML-KEM ciphertexts are **not** stored on-chain. Only a proof commitment, key fingerprint, and issuance epoch land in the contract.

---

## 2. Cryptographic baseline (FIPS 203 / 204 / 205)

NIST published the three primary post-quantum FIPS as **final standards on 13 August 2024**. They are the current federal baseline for quantum-resistant public-key cryptography. Implementations must use the **FIPS algorithm names and parameter-set names**, not the round-3 contest names (Kyber, Dilithium, SPHINCS+).

NIST maintains a [PQC FIPS FAQ](https://csrc.nist.gov/projects/post-quantum-cryptography) (posted 31 January 2025). As of 31 July 2026, FIPS 204 has a published **errata / potential-updates** spreadsheet; production bindings should track those corrections when NIST issues a revision.

### FIPS 204 — ML-DSA (primary signature)

[FIPS 204](https://csrc.nist.gov/pubs/fips/204/final), *Module-Lattice-Based Digital Signature Standard*, specifies **ML-DSA**, derived from CRYSTALS-Dilithium. It is the primary algorithm for authenticating official communications.

| Parameter set | NIST security category | Typical role in PQ-SC-USG |
|---------------|------------------------|---------------------------|
| ML-DSA-44 | Category 2 | Development / constrained pilots only |
| ML-DSA-65 | Category 3 | Unclassified public notices when CNSA 2.0 is not required |
| **ML-DSA-87** | Category 5 | **Default for National Security Systems (CNSA 2.0)** |

**Protocol role:** the authorized USG signer produces an ML-DSA signature over `SHA3-256(document ‖ metadata)`. Verification of that signature is the statement proven inside the zkVM. For NSS and dual-use systems that must meet CNSA 2.0, use **ML-DSA-87 only**; ML-DSA-44 and ML-DSA-65 are not CNSA 2.0 approved.

### FIPS 205 — SLH-DSA (stateless hash-based fallback)

[FIPS 205](https://csrc.nist.gov/pubs/fips/205/final), *Stateless Hash-Based Digital Signature Standard*, specifies **SLH-DSA**, derived from SPHINCS+. Security rests on the collision- and preimage-resistance of SHA-2 or SHAKE, **independent of lattice assumptions**.

| Family | Security target | Notes |
|--------|-----------------|-------|
| SLH-DSA-SHA2-{128,192,256}{s,f} | 128 / 192 / 256-bit | SHA-2 instantiations; `s` = small signature, `f` = fast signing |
| SLH-DSA-SHAKE-{128,192,256}{s,f} | 128 / 192 / 256-bit | SHAKE instantiations |

**Protocol role:** algorithm-diversity fallback when a lattice break would be catastrophic, or when a civilian / unclassified issuer elects hash-based signatures. Signatures are much larger than ML-DSA; they remain **off-chain** and are attested by the same zk-STARK path.

**CNSA 2.0 note:** NSA does **not** approve SLH-DSA for National Security Systems. NSS issuers use ML-DSA-87. Civilian agencies following OMB / CISA PQC guidance may still register an SLH-DSA fingerprint as a secondary algorithm.

### FIPS 203 — ML-KEM (attachment confidentiality)

[FIPS 203](https://csrc.nist.gov/pubs/fips/203/final), *Module-Lattice-Based Key-Encapsulation Mechanism Standard*, specifies **ML-KEM**, derived from CRYSTALS-Kyber. A KEM establishes a shared secret over a public channel; that secret then keys a FIPS-approved AEAD (AES-256-GCM) for attachments.

| Parameter set | NIST security category | Typical role in PQ-SC-USG |
|---------------|------------------------|---------------------------|
| ML-KEM-512 | Category 1 | Not used for USG production |
| ML-KEM-768 | Category 3 | Unclassified attachment encryption when CNSA 2.0 is not required |
| **ML-KEM-1024** | Category 5 | **Default for NSS (CNSA 2.0)** |

**Protocol role:** encapsulate a shared secret to the recipient’s ML-KEM public key before the attachment is placed in IPFS / Arweave. The public issuance record can remain unclassified while attachments stay confidential. Follow [NIST SP 800-227](https://csrc.nist.gov/pubs/sp/800/227/final) for KEM usage (domain separation, hybrid constructions, and binding the encapsulated key to the content hash).

Production USG deployments should use **FIPS 140-3** validated modules (liboqs / vendor CMVP modules) for ML-DSA, SLH-DSA, and ML-KEM. Hybrid classical+PQ signing is allowed during transition; the on-chain proof must still attest the **PQ** signature.

---

## 3. Issuance flow (post-quantum native)

The cryptographic issuance sequence is unchanged; only the settlement interface is TON.

1. An authorized USG signer computes a SHA-3-256 hash of the document plus metadata (timestamp, classification, issuer DID). Poseidon2 is used **inside** the zk circuit as the zk-friendly equivalent.
2. The signer signs that hash **offline** with **FIPS 204 ML-DSA** (primary) or **FIPS 205 SLH-DSA** (fallback). **FIPS 203 ML-KEM** encapsulates a shared secret for hybrid encryption of attachments.
3. An off-chain prover (RISC Zero or Succinct SP1 today; any RISC-V zkVM that emits a STARK) executes full PQ verification inside a minimal zkVM.
4. The prover emits a succinct **zk-STARK** attesting: the signature is valid against the registered PQ public-key fingerprint **and** the content hash matches the declared metadata.
5. Proof plus minimal metadata is submitted to the TON **issuance smart contract** as an external inbound message. The contract verifies the proof and deploys / updates the unique item contract for that communication.

On-chain fields are limited to: PQ key fingerprint, issuance epoch, and proof commitment. Content URI points at IPFS (primary) with an Arweave backup.

---

## 4. On-chain protocol (TON)

TON is the sole settlement chain. Collection and item contracts follow TEP-62; metadata follows TEP-64. New contract code is written in **Tolk** (FunC is legacy). TVM async messaging and sharding support high-volume daily notices at negligible fee.

- Issuance is gated exclusively by successful zk-STARK verification in the collection / verifier contract.
- Each official communication is a unique **item smart contract** under a USG **collection contract** (the TEP-62 unique-item pattern).
- Recursive STARK aggregation batches many issuances (for example, all daily DoD notices) into one on-chain verify.
- Proof submission uses TON external messages (or a thin oracle relay if the verifier is split). No Ethereum account-abstraction path is required.
- TON testnet is the pilot network; mainnet follows audit.

Public verification: any TON light client, explorer, or wallet can check the proof commitment and read the content URI. No secret keys and no quantum-vulnerable primitives are exposed on-chain.

### Why TON (and only TON)

- Sub-cent fees and asynchronous sharding suit high-frequency official notices.
- Native unique-item and collection contracts (TEP-62) map cleanly to “one communication → one smart contract.”
- Telegram-scale public reach without a second settlement chain.
- Cryptographic artifacts (FIPS signatures, STARK proof, commitment) stay chain-local to TON; there is no parallel EVM, SVM, or zkEVM deployment.

---

## 5. Technical mitigations

- Never store raw PQ signatures or ML-KEM ciphertexts on-chain (avoids TVM cell-size and fee blow-ups).
- Prove ML-DSA / SLH-DSA verification inside a RISC-V zkVM; verify only the STARK on TON.
- Modular Verifier Interface (MVI): swap RISC Zero / SP1 proof formats without changing the issuance contract ABI.
- Poseidon2 inside the circuit; SHA-3-256 at the FIPS boundary so signed bytes match NIST-approved hashing.
- Algorithm registry on the collection contract stores fingerprints for ML-DSA-87 (required for NSS) and optional SLH-DSA for diversity.

---

## 6. Abstract (for proposals / whitepaper)

We propose **PQ-SC-USG**, a TON smart-contract protocol for issuing U.S. Government communications as post-quantum-secure on-chain records. Each communication is signed offline with NIST FIPS 204 (ML-DSA) or FIPS 205 (SLH-DSA); FIPS 203 (ML-KEM) protects attachments. A RISC-V zkVM prover generates a succinct zk-STARK attesting signature validity and content integrity against registered USG post-quantum public-key fingerprints. The proof is submitted to a TON issuance smart contract (TEP-62 collection / item), which records an immutable item whose metadata links to permanent off-chain storage (IPFS + Arweave). On-chain verification uses a TVM STARK verifier, delivering quantum-resistant authentication to any public observer without exposing raw post-quantum signatures on-chain.

---

## Documents

| Document | Role |
|----------|------|
| [draft-architecture.md](draft-architecture.md) | Layered architecture (current baseline) |
| [draft-workflow.md](draft-workflow.md) | Functional issuance flow |
| [glossary.md](glossary.md) | Terms and FIPS parameter names |
| [workflow.md](workflow.md) | Architecture infographic brief |
| [archive/](archive/) | Historical timeline, team roles, and multi-chain notes |

**License:** MIT (code) + CC0 (specification)  
**Status:** Pre-alpha specification (TON path)  
**Source baseline:** [Mayweather](https://github.com/thomasBartelsPizzone/Mayweather)

---

*Last updated: August 2026*  
Built for U.S. Government authenticity in a post-quantum world.

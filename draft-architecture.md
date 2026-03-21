# PQ-NFT-USG Protocol Layered-Architecture

**Document ID**: PQ-NFT-USG-ARCH-001  
**Version**: Draft v0.2 (March 2026)  
**Systems Engineering Level**: From Concept → Preliminary Design (PDR ready)  
**Approval Status**: IPT Draft – Pending PDR Gate

## Layer 7 – Application Layer
- USG Issuance (memo / press release / video)
- Metadata package (timestamp, classification, issuer DID)

## 6 – Cryptographic Primitives Layer
- Hash function: SHA-3-256 (fallback) / Poseidon2 (zkVM-optimized)
- Digital Signature: FIPS 204 ML-DSA-44/65/87 (primary), FIPS 205 SLH-DSA (stateless fallback)
- Key Encapsulation: FIPS 203 ML-KEM-512/768/1024 (hybrid)
- Key Registry: On-chain PQ public-key fingerprint (only 32-byte hash)

## Layer 5 – Zero-Knowledge Proving Subsystem

Prover: leanVM-compatible RISC-V zkVM (native target)
- Bridge today: RISC Zero or Succinct SP1
- Proof type: zk-STARK (transparent, hash-only security, no trusted setup)
- Recursion: Built-in 2-to-1 aggregation for daily batch issuance
- leanVM Mitigation Strategy:
   - Modular Verifier Interface (MVI) pattern
   - Abstract Verifier contract with pluggable implementation
   - Today: SP1/RISC Zero precompile stub
   - Tomorrow: Direct leanVM precompile swap (zero code change)


## Layer 4 – Proof Submission & Verification
- Account Abstraction: ERC-4337 (Ethereum) or equivalent external message (TON)
- On-chain verification: O(1) gas via zk-STARK verifier

## Layer 3 – Blockchain Settlement Layer
- NFT Standard: ERC-721/1155 (EVM) or TEP-62/74 (TON)
- On-chain data: Only proof commitment + PQ key fingerprint + issuance epoch

## Layer 2 – Immutable Storage Layer
- Primary: IPFS (content-addressed)
- Permanent backup: Arweave (permaweb)

## Layer 1 – Public Consumption Layer
- Any Ethereum/TON light client, block explorer, or wallet performs instant zk-STARK verification

---
**Core Systems Invariants** (maintained across all deployment chains):

- Quantum-resistant provenance (FIPS 203/204/205 + zk-STARK)
- No raw PQ signatures or keys stored on-chain
- Zero-trust public verification (no trusted third party)
- Recursive batch issuance capability
- Chain-agnostic cryptographic payload (only settlement interface changes)
---
**Next Steps**  
- [ ] Finalize Interface Control Document (ICD) for cryptographic payload  
- [ ] Complete PDR package (this document + SRD + Trade Study)

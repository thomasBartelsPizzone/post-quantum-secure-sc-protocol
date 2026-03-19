# PQ-NFT-USG: Post-Quantum NFT Protocol for USG Official Communications

**Repository Purpose**  
Production-grade, open-source protocol specification + reference implementation for issuing U.S. Government communications (press releases, speeches, troop-deployment notices, etc.) as immutable, publicly verifiable NFTs secured against both classical and quantum adversaries.

**Primary Target Chain**: Ethereum (EVM-compatible)  
**Secondary / Alternative Chains Evaluated**: TON (Telegram Open Network), Solana, Polygon zkEVM, and custom L1 options.

---

## 1. Reviewed & Polished High-Level Concept

Your original vision — Ethereum-based smart contracts + NFTs for USG communications with post-quantum security via FIPS 203/204/205, zk-STARKs, and leanVM — is technically sound and aligns with Ethereum’s 2025–2026 “lean Ethereum” roadmap.

### Core Protocol Name  
**PQ-NFT-USG** – Post-Quantum NFT Protocol for USG Official Communications

### Issuance Flow (Post-Quantum Native)
1. Authorized USG signer computes SHA-3/Poseidon2 hash of the document + metadata (timestamp, classification, issuer DID).  
2. Signs the hash offline using **FIPS 204 ML-DSA** (primary) or **FIPS 205 SLH-DSA** (fallback). **FIPS 203 ML-KEM** for hybrid encryption of attachments.  
3. Off-chain prover (leanVM-compatible or RISC Zero/SP1 bridge) executes full PQ verification logic inside a minimal zkVM.  
4. Generates a succinct **zk-STARK proof** attesting: signature is valid against registered PQ public-key fingerprint AND content hash matches declared metadata.  
5. Proof + minimal metadata is submitted to the smart contract (via ERC-4337 account abstraction for PQ-native wallets).

### On-Chain Protocol (Ethereum Primary)
- NFT minting gated exclusively by successful zk-STARK proof verification.  
- leanVM verifier (precompile or contract) checks proof in near-constant gas.  
- TokenURI points to immutable storage (IPFS CID + Arweave backup).  
- On-chain fields: only PQ key fingerprint, issuance epoch, proof commitment.  
- Recursive STARK aggregation for batch issuance (e.g., all daily DoD notices in one proof).

### Public Verification
Any Ethereum light client, explorer, or wallet instantly verifies authenticity — no secret keys, no quantum-vulnerable primitives exposed on-chain.

### Key Technical Mitigations
- Never stores raw PQ signatures on-chain (avoids gas explosion).  
- Uses leanVM’s 5-instruction RISC-V ISA + Poseidon2 for prover efficiency.  
- Fully compatible today via RISC Zero/SP1 bridges; natively optimal once leanVM lands in 2027+.

---

## 2. Alternative Chain Approaches (Beyond Ethereum)

We evaluated four viable alternatives. Ethereum remains the **recommended primary** due to ecosystem maturity, tooling, and USG familiarity, but the protocol is deliberately designed to be chain-agnostic at the cryptographic layer.

| Chain          | Compatibility | Pros                                                                 | Cons                                                                 | Recommended Use Case                          | Migration Effort |
|----------------|---------------|----------------------------------------------------------------------|----------------------------------------------------------------------|-----------------------------------------------|------------------|
| **Ethereum (L2)** | Native (EVM + leanVM roadmap) | Mature dev tools, ERC-721/4337, huge liquidity, public auditability | Gas costs until leanVM; L2 fragmentation                     | Primary – all USG pilots                      | Baseline (0)     |
| **TON (Telegram Open Network)** | TVM (FunC) – not EVM | Extremely cheap fees (~$0.01), async sharding (millions TPS), native high-throughput, built-in Jetton/NFT standards | No native zk-STARK/leanVM; would require custom FunC verifier or off-chain proof relay. TON’s TVM is Turing-complete but lower-level. | High-volume daily issuances (DoD notices)     | Medium (rewrite contracts in FunC + custom STARK verifier) |
| **Solana**     | SVM (Rust)    | Sub-second finality, < $0.001 fees, native program composability     | No native zk-STARK support (requires Light Protocol or custom ZK); different account model | Real-time operational notices                 | Medium-High (Rust programs + custom ZK integration) |
| **Polygon zkEVM / zk-rollups** | EVM-compatible | Lower fees than L1, already zk-native (Polygon zkEVM + CDK)          | Still depends on Ethereum settlement; less mature PQ roadmap     | Cost-optimized Ethereum fork                  | Low (near drop-in) |

**TON-Specific Adaptation Path (Recommended Secondary)**  
- Use TON’s native NFT standard (TEP-62/TEP-74) instead of ERC-721.  
- Off-chain zk-STARK proof generation remains identical (FIPS + leanVM circuit).  
- Submit proof via TON’s external message or oracle bridge to a FunC smart contract that verifies the STARK (or uses a lightweight validator).  
- Advantage: TON’s async model allows 10,000+ issuances/day at negligible cost — ideal for high-frequency USG notices.  
- Reference: TON already powers massive Telegram-based apps; adding PQ-NFT layer is feasible in 2026–2027.

**Recommendation**:  
Start with **Ethereum L2 (Base or Arbitrum)** for the pilot (maximum developer velocity and public verifiability).  
Add **TON parallel deployment** as Phase 2 (after Ethereum MVP) for cost-sensitive, high-volume use cases. All cryptographic primitives (FIPS signatures + zk-STARK circuits) remain identical across chains.

---

## 3. Abstract (for proposals / whitepaper)

We propose PQ-NFT-USG, an efficient, chain-agnostic smart-contract protocol for issuing U.S. Government communications as post-quantum-secure NFTs. Each communication is signed offline with NIST FIPS 204 (ML-DSA) or FIPS 205 (SLH-DSA) and FIPS 203 (ML-KEM) for encryption. A leanVM-based prover generates a succinct zk-STARK proof attesting signature validity and content integrity against registered USG post-quantum public-key fingerprints. The proof is submitted to an NFT-gated smart contract (ERC-721 on Ethereum or TEP-62 on TON), which mints an immutable NFT whose metadata links to permanent off-chain storage (IPFS + Arweave). On-chain verification uses the leanVM zk-STARK verifier, delivering constant-time, quantum-resistant authentication to any public observer.

The protocol leverages Ethereum’s leanVM roadmap (or TON’s high-throughput TVM) to keep gas/fees practical despite larger PQ key sizes. It provides tamper-proof, publicly auditable provenance for official communications while preserving forward secrecy and zero-downtime migration from current cryptography.

---

## 4. Rough Outline of Deliverables  
**(Intent: Deliver the full protocol — not just a concept)**

Estimated 6–9 months for a focused team (or 22–28 months part-time @ 10 h/week × 3 engineers).

1. **Protocol Specification & Architecture** (Months 1–2)  
   - Formal spec, security model, threat analysis, leanVM/TON migration paths.  
   - Metadata schema and governance for issuer-key registration.

2. **Cryptographic Primitives & ZK Components** (Months 2–4)  
   - FIPS 203/204/205 bindings + zk-STARK prover (leanVM-compatible).  
   - Recursive aggregation circuits.  
   - Test vectors and formal security proofs.

3. **Smart-Contract Suite & Reference Implementation** (Months 3–5)  
   - Ethereum (Solidity) + TON (FunC) reference contracts.  
   - Issuance CLI + video-generation-to-NFT pipeline.  
   - Deployable on testnets today.

4. **Testnet Deployment & Demo Suite** (Months 5–6)  
   - Live Ethereum L2 + TON testnet deployments.  
   - Public verifier dashboard and sample USG feeds.

5. **Security Audit, Formal Verification & Hardening** (Months 6–7)  
   - Two independent audits + formal verification (Lean 4/Coq).  
   - Quantum-security analysis.

6. **Final Release & Handover** (Months 7–9)  
   - Complete GitHub repo (MIT/Apache 2.0).  
   - Documentation, integration guide, USG-adoption playbook.

---

## Next Steps

- [ ] Clone this repo and review the `/spec` folder (coming in Phase 1).  
- [ ] Choose primary chain (Ethereum L2 recommended).  
- [ ] Run the learning plan (see `docs/phase1-learning.md`).  

**License**: MIT (code) + CC0 (specification)  
**Status**: Pre-alpha specification (ready for team onboarding)  
**Questions?** Open an issue or contact the maintainer.

---

*Last updated: March 2026*  
Built for U.S. Government authenticity in a post-quantum world.

# PQ-NFT-USG Architecture

## 1. High-Level Architecture (CONOPS View)

The system is a **post-quantum authenticated issuance pipeline** that transforms any official USG communication (text memo, speech transcript, or video) into an immutable, publicly verifiable NFT.

**Top-Level Flow**
USG Authorized Signer → PQ Signing (FIPS 203/204/205) → zk-STARK Proof Generation (leanVM-compatible) → On-Chain Verification & Mint → Immutable Storage (IPFS + Arweave) → Public Consumption (any light client).

**Key Functional Blocks**
- **Issuance Layer** (off-chain)
- **Cryptographic Assurance Layer** (FIPS + zk-STARK)
- **Settlement Layer** (blockchain-specific smart contract)
- **Verification & Consumption Layer** (public read-only)

**Primary Chain Recommendation**  
Ethereum L2 (Base/Arbitrum) – maximum developer velocity and public auditability.

## 2. Detailed Architecture (Functional Decomposition & Trade Study)

### Functional Decomposition (Systems Engineering View)
1. **Input Interface** – USG document/video + metadata (timestamp, classification, issuer DID)
2. **Hash & Signature Module** – SHA-3/Poseidon2 hash → FIPS 204 ML-DSA (primary) or FIPS 205 SLH-DSA (fallback); FIPS 203 ML-KEM for attachments
3. **Proof Generation Module** – leanVM/RISC Zero/SP1 zkVM circuit performs full PQ verification logic; recursive STARK aggregation for batch issuances
4. **Settlement Module** (chain-specific)
   - Ethereum: ERC-721/ERC-4337 smart contract with leanVM verifier stub
   - TON: TEP-62/TEP-74 Jetton/NFT contract + FunC verifier (or oracle-relayed proof)
5. **Storage Module** – TokenURI → IPFS CID + Arweave permanent backup
6. **Verification Module** – On-chain leanVM verifier (O(1) gas) + public light-client interface

### Chain Trade Study (Updated March 2026)

| Chain | Settlement Model | Fees per Issuance | Native ZK Support | PQ Migration Path | Recommendation Score (1–10) |
|-------|------------------|-------------------|-------------------|-------------------|-----------------------------|
| **Ethereum L2** | EVM + leanVM roadmap | $0.05–0.30 (L2) | High (SP1/leanVM bridge) | Native 2027+ | **9.5** (primary) |
| **TON** | TVM (FunC) | <$0.01 | Medium (custom FunC verifier or relay) | High throughput, async | **8.0** (secondary) |
| **Solana** | SVM (Rust) | <$0.001 | Low (Light Protocol) | Requires custom program | 6.5 |
| **Polygon zkEVM** | zk-EVM | $0.01–0.10 | High | Near drop-in | 7.5 |

**Migration Strategy**  
All cryptographic primitives (FIPS signatures + zk-STARK circuits) are **chain-agnostic**. Only the settlement smart-contract layer changes. Interface Control Document (ICD) defines a common JSON proof payload that is consumed by any target chain’s verifier contract.

**leanVM Mitigation Approaches**  
- Bridge mode (today): RISC Zero / SP1 zkVM with identical RISC-V circuit  
- Native mode (2027+): Swap verifier precompile via upgradeable proxy pattern  
- Hybrid fallback: Stateless SLH-DSA verification if leanVM unavailable

**Detailed Data Flow Diagram** (text version)

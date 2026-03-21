# PQ-NFT-USG Protocol Architecture

**Document ID**: PQ-NFT-USG-ARCH-001  
**Version**: Draft v0.2 (March 2026)  
**Systems Engineering Level**: From Concept → Preliminary Design (PDR ready)  
**Approval Status**: IPT Draft – Pending PDR Gate

## 1. High-Level Architecture (Functional Flow)

```mermaid
graph TD
    subgraph "USG Issuance Domain"
        A[USG Authorized Signer] -->|offline| B[Official Communication
Memo / Press Release / Video]
        B --> C["Metadata Package
(timestamp, classification, issuer DID)"]
    end

    C --> D[Compute Hash
SHA-3-256 / Poseidon2]
    D --> E["FIPS 203/204/205 PQ Signature
ML-DSA primary • SLH-DSA fallback
ML-KEM hybrid encryption"]
    
    E --> F[leanVM / RISC Zero / SP1 Prover
RISC-V zkVM environment]
    F --> G["Succinct zk-STARK Proof
Transparent, recursive 2-to-1 aggregation"]
    
    G --> H[Chain-Agnostic Proof Submission
ERC-4337 or TVM external message]
    H --> I[Smart Contract Verifier
Modular verifier interface]
    I --> J[Immutable NFT Mint
ERC-721 / ERC-1155 or TEP-62 / TEP-74]
    
    J --> K[TokenURI → IPFS + Arweave Permanent Storage]
    K --> L[Public Verification Layer
Any light client / explorer / wallet]

    style A fill:#1e3a8a,stroke:#60a5fa,color:#fff
    style L fill:#166534,stroke:#4ade80,color:#fff
    style G fill:#4338ca,stroke:#a5b4fc,color:#fff

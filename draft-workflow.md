# PQ-SC-USG Protocol Architecture

**Document ID:** PQ-SC-USG-ARCH-001  
**Version:** Draft v0.3 (August 2026)  
**Settlement chain:** TON only  
**Systems Engineering Level:** Concept → Preliminary Design (PDR ready)  
**Approval Status:** IPT Draft – Pending PDR Gate

See also: [draft-architecture.md](draft-architecture.md)

## 1. High-level architecture (functional flow)

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
    D --> E["FIPS 203/204/205
ML-DSA-87 primary • SLH-DSA fallback
ML-KEM-1024 attachment wrap"]

    E --> F[RISC Zero / SP1 Prover
RISC-V zkVM environment]
    F --> G["Succinct zk-STARK Proof
Transparent, recursive 2-to-1 aggregation"]

    G --> H[TON External Inbound Message]
    H --> I[Issuance Smart Contract
TEP-62 collection verifier]
    I --> J[Unique Item Smart Contract
TEP-62 item + TEP-64 metadata]

    J --> K[Content URI → IPFS + Arweave]
    K --> L[Public Verification Layer
Any TON light client / explorer / wallet]

    style A fill:#1e3a8a,stroke:#60a5fa,color:#fff
    style L fill:#166534,stroke:#4ade80,color:#fff
    style G fill:#4338ca,stroke:#a5b4fc,color:#fff
    style I fill:#0f766e,stroke:#5eead4,color:#fff
```

# Team Roles & Responsibilities  
**PQ-NFT-USG Project** (3-person core team)

The team is structured using classic systems-engineering roles with clear allocation of requirements, verification, and configuration management responsibilities.

## 1. Back-End Engineer (Middleware & Communication Protocols)
**Primary Focus**  
- Requirements analysis and allocation for the cryptographic and blockchain interface layers  
- Design and implementation of middleware (issuance CLI, proof submission pipeline, cross-chain relay adapters)  
- Communication-protocol definition (JSON proof payload ICD, ERC-4337 user-operation format, TON external-message schema)  

**Research Responsibilities**  
- Cryptographic primitives integration (FIPS 203/204/205 binding into Rust/RISC-V circuits)  
- zk components (STARK circuit authoring in SP1/leanVM ISA)  
- leanVM mitigation approaches (bridge vs. native verifier trade study and upgradeable proxy design)  

**Additional Duties**  
- Authoring of protocol specification sections related to interfaces and data flows  
- Drafting of solution architecture diagrams and functional decomposition  
- **Future Role Evolution**: May assume Project Manager / Chief Architect responsibilities (technical baseline control, trade-study leadership) starting Phase 3.

## 2. Cybersecurity Admin
**Primary Focus**  
- Security requirements development and verification  
- Threat modeling, risk assessment, and mitigation planning (quantum and classical attack vectors)  
- Key-management CONOPS and governance model for USG issuer public-key registry  

**Research Responsibilities**  
- Cryptographic primitives security analysis (side-channel resistance, hybrid classical/PQ schemes)  
- zk components soundness (zk-STARK security proofs, recursion depth analysis)  
- leanVM mitigation approaches from a security viewpoint (trusted vs. transparent setups, formal verification of verifier contract)  

**Additional Duties**  
- Co-authoring of protocol specification security model and threat-analysis sections  
- Review and approval of all solution-architecture drafts for compliance with CISA/NSA PQ guidance and FedRAMP considerations.

## 3. Manufacturing Productivity Engineer (with DBA & Dev Experience)
**Primary Focus**  
- Project planning, scheduling, and Control Account Management (CAM) functions  
- Database and storage architecture (metadata schema, IPFS/Arweave integration, on-chain indexing)  
- Process optimization and productivity metrics (gas-cost modeling, proof-generation throughput, hardware utilization)  

**Research Responsibilities**  
- Support cryptographic primitives and zk components research through benchmarking and performance data collection  
- Document leanVM mitigation approaches from a systems-integration and deployment perspective  

**Additional Duties**  
- Lead authoring of protocol specification sections on storage, performance, and deployment  
- Draft and maintain solution-architecture configuration baselines and interface control documents  
- **Future Role Evolution**: May assume Control Account Manager (CAM) responsibilities (Earned Value Management, cost/schedule variance tracking) starting Phase 2.

## Systems-Engineering Practices Used
- **CONOPS** – High-level Concept of Operations document (see `02-architecture…md`)  
- **Requirements Allocation Matrix** – Maintained in `/spec/requirements.md`  
- **Interface Control Documents (ICD)** – Proof payload and chain-adapter interfaces  
- **Trade Studies** – Chain selection, zkVM bridge options, proof-generation hardware  
- **Verification & Validation Plan** – Linked to every research artifact and architecture draft  
- **Configuration Management** – GitHub + pull-request reviews with mandatory Cyber Admin approval for security sections

**Team Size**  
Strictly limited to these three roles for Phase 1–2. External consultants (PQ cryptographer mentor, independent auditor) engaged only on an as-needed basis.

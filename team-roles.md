# docs/team-roles-and-responsibilities.md

# Team Roles & Responsibilities  
**PQ-NFT-USG Systems Engineering Team**  
**Team Size**: Exactly 3 (Integrated Product Team – IPT)  
**Document Status**: Baseline v0.1 (March 2026)

## 1. Equipment Integration Engineer
**Current Role**: Technical Lead – Middleware Integration  
**Focus Areas**:
- Systems Integration of cryptographic primitives with off-chain provers
- Communication protocol design (ERC-4337 account abstraction, external message relays for TON)
- zk-STARK circuit implementation and recursive aggregation
- leanVM mitigation approaches (modular verifier interface, RISC Zero/SP1 bridge)
- Smart-contract development (Solidity / FunC)

**Future Evolution**: May transition to **Chief Architect** (Phase 3+) responsible for overall technical baseline, interface control documents (ICDs), and Configuration Management (CM).

## 2. Cybersecurity Admin
**Current Role**: Information Assurance & Cryptographic Authority  
**Focus Areas**:
- Research and validation of cryptographic primitives (FIPS 203/204/205, liboqs/PQClean bindings)
- Security requirements definition (SRD) and threat analysis (quantum + classical)
- zk component verification (STARK soundness proofs, side-channel resistance)
- leanVM mitigation security assessment
- Risk Management Board (RMB) participation

## 3. Manufacturing Productivity Engineer
**Current Role**: Systems Analyst & Control Account Support  
**Focus Areas**:
- Research authoring and drafting of protocol specification
- Solution architecture drafts (high-level & detailed views)
- Database / storage layer design (IPFS + Arweave integration)
- Project planning, scheduling, and Earned Value Management (EVM) tracking
- Benchmarking, trade studies, and verification planning

**Future Evolution**: May transition to **Control Account Manager (CAM)** (Phase 2+) responsible for cost, schedule, and technical performance baselines under Earned Value Management System (EVMS).

## Systems Engineering Responsibilities Matrix (RACI)

| Activity | Equipment Integration Engineer | Cybersecurity Admin | Manufacturing Productivity Engineer |
|----------|--------------------------------|---------------------|-------------------------------------|
| Cryptographic primitives research | C | A | R |
| zk components & leanVM mitigations | A | R | C |
| Protocol specification authoring | C | R | A |
| Solution architecture drafts | R | C | A |
| Trade studies & benchmarking | C | R | A |
| PDR package preparation | A | C | R |

**Legend**: R = Responsible, A = Accountable, C = Consulted

**Governance Note**: The IPT operates under a single Systems Engineering Management Plan (SEMP). Weekly Technical Interchange Meetings (TIMs) and monthly Risk & Opportunity Reviews ensure alignment with project baselines.
## Systems-Engineering Practices Used
- **CONOPS** – High-level Concept of Operations document (see `02-architecture…md`)  
- **Requirements Allocation Matrix** – Maintained in `/spec/requirements.md`  
- **Interface Control Documents (ICD)** – Proof payload and chain-adapter interfaces  
- **Trade Studies** – Chain selection, zkVM bridge options, proof-generation hardware  
- **Verification & Validation Plan** – Linked to every research artifact and architecture draft  
- **Configuration Management** – GitHub + pull-request reviews with mandatory Cyber Admin approval for security sections

**Team Size**  
Strictly limited to these three roles for Phase 1–2. External consultants (PQ cryptographer mentor, independent auditor) engaged only on an as-needed basis.

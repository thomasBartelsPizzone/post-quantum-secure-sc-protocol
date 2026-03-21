# Phase 1 – Learning, Research & Knowledge Ramp-Up Timeline

**Phase Duration** → Months 1–4 (16 weeks)  
**Total Team Effort** → 600 person-hours (3 engineers × ≤10 h/week)  
**Objective** → Achieve baseline proficiency in post-quantum cryptography, zero-knowledge systems, and target blockchain platforms so that subsequent phases produce verifiable, production-grade artifacts.
**Systems Engineering Objective**: Complete Systems Requirements Definition (SRD) and Technology Readiness Level (TRL) assessment for cryptographic primitives, zk components, and leanVM mitigation strategies before proceeding to Preliminary Design Review (PDR).

## Weekly Timeline & Deliverables

| Week | Focus Area | Primary Activities | Key Research Outputs | Owner | Hours |
|------|------------|--------------------|----------------------|-------|-------|
| 1–2 | Blockchain Foundations | Solidity + Foundry (Ethereum), FunC basics (TON), TVM overview | Working Sepolia ERC-721 mint + TON Jetton test | All | 60 |
| 3–4 | Ethereum Ecosystem & ZK Intro | EVM gas model, ERC-4337, RISC Zero/SP1 tutorials | First zk-STARK "hello world" circuit + gas benchmarks | Backend Eng | 60 |
| 5–6 | Post-Quantum Cryptography | NIST FIPS 203/204/205 specs, liboqs/PQClean bindings | Offline ML-DSA signing/verification demo in Rust | Cyber Admin | 60 |
| 7–8 | zkVM & STARK Circuits | leanVM whitepapers, RISC-V zkVM execution model, FRI protocol | Minimal ML-DSA verification circuit in SP1/RISC Zero | All | 60 |
| 9–10 | Chain Trade Studies | TON TVM vs. EVM performance, Solana SVM comparison | Chain-selection trade-study matrix (TPS, fees, ZK support) | Manufacturing Eng | 60 |
| 11–12 | Benchmarking & Mitigation Research | Proof generation on RTX 4090 hardware, leanVM bridge options | Benchmark report (proof time, gas, cost per issuance) | Backend Eng | 60 |
| 13–14 | Security & Standards | CISA/NSA PQ migration guidance, FedRAMP considerations | Threat model draft + key-management CONOPS | Cyber Admin | 60 |
| 15–16 | Synthesis & Documentation | Integrate all research; internal peer review | Phase 1 Research Notebook + leanVM mitigation whitepaper | All | 120 |

| Week | 1–2          | 3–4          | 5–6          | 7–8          | 9–10         | 11–12        | 13–14        | 15–16        |
|------|--------------|--------------|--------------|--------------|--------------|--------------|--------------|--------------|
| **Solidity & Ethereum Foundations** | ██████████ | ████         |              |              |              |              |              |              |
| **Ethereum PQ & leanVM Landscape Research** |              | ██████████ | ██████       |              |              |              |              |              |
| **Post-Quantum Crypto (FIPS 203/204/205 + liboqs)** |              |              | ██████████ | ██████████ |              |              |              |              |
| **zkVMs, STARK Circuits & Recursion** |              |              |              | ██████       | ██████████ | ██████       |              |              |
| **Industry Benchmarking & Trade Studies** |              |              |              |              |              | ██████████ | ██████       |              |
| **Spec Drafting (Security Model, Metadata Schema, Chain Migration Paths)** |              |              |              |              |              |              | ██████████ | ██████████ |
| **Milestones** | First testnet mint | Dev env + RISC Zero/SP1 setup | First FIPS 204 demo | First zk-STARK proof of ML-DSA | Benchmark report v0.5 | Full research notebook | Spec Draft v0.8 | PDR Package Complete |

**Legend**: █ = active work (proportional to effort)


**Project ID**: PQ-NFT-USG-Phase1  
**Baseline Start**: 2026-03-21  
**Baseline Finish**: 2026-07-12  
**Earned Value Management (EVM) Baseline**: 600 hours

| Task ID | Task Name | Duration (h) | Start | Finish | Predecessors | Resources | % Complete (planned) | Notes / SE Deliverable |
|---------|-----------|--------------|-------|--------|--------------|-----------|----------------------|------------------------|
| 1.0 | Phase 1 Kickoff & Team Alignment | 20 | 2026-03-21 | 2026-03-24 | — | All | 0% | Systems Engineering Management Plan (SEMP) |
| 1.1 | Solidity & Ethereum Foundations | 60 | 2026-03-25 | 2026-04-07 | 1.0 | All | 0% | ERC-721/4337 testnet deployment |
| 1.2 | Ethereum PQ & leanVM Landscape Research | 60 | 2026-04-08 | 2026-04-21 | 1.1 | All | 0% | leanVM TRL Assessment Report |
| 1.3 | Post-Quantum Crypto Training & Demos | 120 | 2026-04-22 | 2026-05-19 | 1.2 | BE + Cyber | 0% | FIPS 204/205 offline signing prototype |
| 1.4 | zkVM / STARK Circuit Development | 120 | 2026-05-20 | 2026-06-09 | 1.3 | BE + Cyber | 0% | Recursive STARK proof-of-concept |
| 1.5 | Industry Benchmarking & Chain Trade Studies | 80 | 2026-06-10 | 2026-06-23 | 1.4 | All | 0% | Benchmark Report + Trade Study Matrix |
| 1.6 | Protocol Specification Drafting | 120 | 2026-06-24 | 2026-07-12 | 1.5 | All | 0% | SRD v0.9 + Architecture Drafts |
| 1.7 | Phase 1 PDR Gate Review | 20 | 2026-07-13 | 2026-07-13 | 1.6 | All | 0% | PDR Package + Risk Register Update |

**Critical Path**: 1.0 → 1.1 → 1.2 → 1.3 → 1.4 → 1.5 → 1.6 → 1.7  
**Total Float**: 0 days on critical path  
**Resource Loading**: Back-end Engineer (45%), Cybersecurity Admin (35%), Manufacturing Productivity Engineer (20%)

**Cumulative Deliverables at Week 16**
- Research Notebook (GitHub repo `/research/`)
- Chain Trade-Study Matrix (Markdown + Excel export)
- Benchmark Report v1.0 (proof-generation timings + hardware specs)
- Draft Threat Model & PQ Key-Management CONOPS
- Team self-assessment scorecard (proficiency levels per topic)

**Success Criteria**  
Every team member can independently generate and verify a zk-STARK proof of an FIPS 204 signature on test hardware.

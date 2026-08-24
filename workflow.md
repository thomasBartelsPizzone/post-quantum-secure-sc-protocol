# PQ-S-SC-USG Protocol Workflow
**Document**: workflow.md  
**Status**: Draft – TON-only architecture  
**Last Updated**: 24 August 2026

---

## High-Level Flow

1. **USG Issuance**  
   Authorized signer creates the official communication (memo / press release / video) + metadata.

2. **Cryptographic Primitives**  
   - Compute SHA-3 / Poseidon2 hash of content + metadata  
   - Offline PQ signing using FIPS 204 (ML-DSA) or FIPS 205 (SLH-DSA) + FIPS 203 (ML-KEM)

3. **ZK Proving (leanVM / RISC-V)**  
   Generate a succinct, transparent zk-STARK proof (with recursive aggregation for batch issuance).

4. **Proof Submission**  
   Submit the proof + minimal metadata to TON via external message.

5. **Blockchain Settlement (TON)**  
   - TON smart contract (FunC / TVM)  
   - Mint NFT using TEP-62 / TEP-74  
   - Support recursive STARK batching

6. **Immutable Storage**  
   TokenURI points to IPFS + Arweave for permanent storage.

7. **Public Verification & Consumption**  
   Any TON light client, wallet, or explorer can instantly verify the zk-STARK proof and read/watch the content.

---

## Key Design Decisions (Current)

- **Target chain**: TON (high-volume / low-fee focus)
- **Smart contract language**: FunC / TVM
- **NFT standard**: TEP-62 / TEP-74
- **Proof system**: zk-STARK (transparent, post-quantum)
- **Prover**: leanVM-compatible RISC-V zkVM (with open-source bridge options)
- **Storage**: IPFS (primary) + Arweave (permanent backup)
- **All tools**: Open-source preferred; local compute where possible

---

## Notes

- Cryptographic payload remains chain-agnostic so future multi-chain support is still possible.
- Recursive STARK aggregation is designed for daily / high-volume USG issuance batches.

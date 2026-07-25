# Proposed Architecture — SetuExam (working name)

_A blockchain-native, cryptographically-verifiable exam pipeline that a serious government stakeholder could actually pilot._

> **Setu** (सेतु) = bridge. This is a bridge between the current bureaucratic exam ops and a cryptographically-guaranteed one — no rip-and-replace.

## Design principles (non-negotiable)

1. **No single custodian.** No party — not NTA, not TCS, not the government — should be able to leak the paper alone.
2. **Publicly verifiable, privacy-preserving.** Any citizen (student, media, court) can verify the process without seeing the paper.
3. **Retrofit-friendly.** Runs alongside existing NTA/TCS infrastructure; no big-bang migration.
4. **Offline-capable at centre.** Verifications work without internet (rural centres, network attacks).
5. **Politically buyable.** Maps directly to Radhakrishnan Committee recs + Unfair Means Act, 2024.

## System roles

| Role | Who | Cryptographic identity |
|---|---|---|
| **Candidate** | Student | DID (`did:web`, `did:key`, or `did:polygon`) in DigiLocker wallet |
| **NTA** | Exam authority | DID `did:web:nta.ac.in` — holds one threshold share |
| **Independent Auditor** | CAG / e.g. IIT dean panel | DID — holds one share |
| **Judicial Observer** | Retd. SC judge panel | DID — holds one share |
| **State Board** | State edu board | DID — holds one share |
| **Civil Society Verifier** | Rotating academic panel | DID — holds one share |
| **Centre** | Exam centre | DID + hardware attestation |
| **Question Author** | Subject expert | DID (anonymised on-chain) |
| **Verifier** (public) | Any citizen | Public read of chain |

_Threshold t=3-of-5 across the middle five roles. Any 3 together can decrypt; any 2 cannot._

## Layered stack

```
┌──────────────────────────────────────────────────────────┐
│  L6  Public verifiability layer                          │
│      Anyone can verify: hashes, signatures, timings      │
├──────────────────────────────────────────────────────────┤
│  L5  Application: candidate app, invigilator app,        │
│      authority ceremony app, auditor dashboard           │
├──────────────────────────────────────────────────────────┤
│  L4  Smart contracts (Polygon)                            │
│      - ExamRegistry, PaperCommitment, ThresholdCeremony  │
│      - CandidateRegistry (DIDs), CentreRegistry           │
│      - AuditLog, KeyRelease                              │
├──────────────────────────────────────────────────────────┤
│  L3  Cryptographic core                                  │
│      Threshold BLS + timed-release (drand tlock)          │
│      VRF for per-candidate shuffle                        │
│      BBS+ selective disclosure for VCs                    │
├──────────────────────────────────────────────────────────┤
│  L2  Identity: W3C DIDs + VCs, wrapped over DigiLocker    │
├──────────────────────────────────────────────────────────┤
│  L1  Storage: encrypted IPFS + regional CDN mirror        │
│      (ciphertext only, non-secret)                       │
└──────────────────────────────────────────────────────────┘
```

## The end-to-end flow (single reference exam)

### Phase 0 — Setup (T-90 days)
- 5 authorities run **Distributed Key Generation (DKG)**: threshold BLS keypair generated; each holds a share; combined public key `PK_exam` published on-chain.
- Question authoring begins in air-gapped facility. Each question hashed and committed to `PaperCommitment` contract (hash tree of the bank).

### Phase 1 — Candidate onboarding (T-90 → T-30)
- Candidate applies → NTA issues **Candidate VC** (signed with NTA DID) into DigiLocker.
- School issues **Educational VC**. UIDAI issues **Aadhaar Face VC** (hash of face template, not template itself). Optional coaching VC.
- Candidate DID + face-hash committed to `CandidateRegistry`.

### Phase 2 — Paper sealing (T-14 days)
- Question bank encrypted under `PK_exam` (threshold BLS ciphertext).
- Ciphertext pinned to IPFS; CID committed to `PaperCommitment`.
- Ciphertext mirrored to every centre via regional CDN + optional DVD. **Fully public artefact — safe to leak.**

### Phase 3 — Per-candidate shuffle commitment (T-1 hour)
- For each candidate, NTA computes `shuffle_seed_i = VRF(NTA_sk, DID_i, exam_id)`.
- Vector of `(DID_i, shuffle_commitment_i)` published on-chain.
- Actual shuffle applied at decrypt time — no advantage from leaking shuffle.

### Phase 4 — Threshold ceremony (T-0)
- Live-streamed. 3-of-5 authorities each submit their partial decryption to `ThresholdCeremony` contract.
- Contract combines partials → releases combined threshold signature/key.
- Every centre's decryption gateway fetches key + decrypts local ciphertext.
- **Impossible before T-0** unless 3 authorities collude publicly.

### Phase 5 — At the centre
- Candidate arrives → invigilator app requests **presentation** of 3-of-4 VCs + live face scan.
- Face template compared to hash inside VC (not to any central DB).
- Verification result signed by centre DID + written to `AuditLog` (anonymised: only DID hash, timestamp, centre).
- Candidate takes CBT in Safe Exam Browser variant with per-candidate shuffled paper.

### Phase 6 — Answer submission
- Answer sheet encrypted client-side; hash committed on-chain; ciphertext to IPFS.
- Enables later verifiable grading without trusting the storage layer.

### Phase 7 — Grading + result
- Grading service runs under attestation; result signed and committed.
- Result issued as VC to candidate's DigiLocker.
- Candidate can present result VC to universities in zero-knowledge (prove above cutoff without revealing rank, etc.).

## Component-by-component build plan (prototype)

| # | Component | Tech | Effort | Priority |
|---|---|---|---|---|
| 1 | Smart contracts | Solidity on Polygon Mumbai | 2 weeks | P0 |
| 2 | DKG + threshold BLS ceremony | Go, `dedis/kyber` or `drand` | 2 weeks | P0 |
| 3 | Paper sealer + IPFS uploader | Rust or Node, `ipfs-http-client` | 1 week | P0 |
| 4 | Candidate wallet (VC store) | React Native + Veramo | 3 weeks | P1 |
| 5 | Invigilator app (VC verifier) | Electron + Veramo + WebRTC face | 3 weeks | P1 |
| 6 | Centre decryption gateway | Rust service, offline-capable | 2 weeks | P0 |
| 7 | VRF-based shuffle | `ecvrf` lib | 3 days | P1 |
| 8 | Public verifier UI | Next.js + wagmi | 1 week | P2 |
| 9 | Auditor dashboard | Next.js + The Graph | 1 week | P2 |
| 10 | Grading service (attested) | Rust + SGX or Nitro | 2 weeks | P2 |

**Prototype goal:** phases 0–5 end-to-end with a fake exam (10 questions, 50 candidates, 3 centres, 5 authorities). Demo the T-0 ceremony live.

## What we intentionally do NOT build (yet)

- **Not** a proctoring AI — use existing (SEB + off-the-shelf).
- **Not** a new blockchain — use Polygon; migrate later if needed.
- **Not** a new identity system — wrap DigiLocker + UIDAI Face API.
- **Not** a grading algorithm — pluggable.

## Where blockchain earns its keep (spelled out)

| Question | Blockchain answer |
|---|---|
| "How do we prove the paper wasn't accessed early?" | Every access event is a tx; timestamps are consensus-anchored |
| "How do we prevent NTA from silently swapping questions?" | Bank hash committed on-chain at T-14; final paper must chain to it |
| "How do we prove the threshold ceremony actually happened?" | Partial decryptions are on-chain, cryptographically verifiable |
| "How do we let students verify without giving them the paper?" | Merkle proofs — verify commitment without content |
| "How do we make audit logs tamper-proof?" | On-chain by construction |
| "How do we distribute trust across independent parties?" | Multi-sig / threshold + independent DIDs |

## Where blockchain is NOT the answer

| Question | Real answer |
|---|---|
| "Can a candidate photograph the screen?" | Physical proctoring, camera detection AI |
| "Can a solver still take the exam with a modified face?" | Multi-issuer VC (harder than corrupting one Aadhaar centre) |
| "Can the centre's LAN be hijacked?" | Mutual TLS + hardware attestation |
| "Can the entire NTA + auditor + judiciary + state + civil-society collude?" | Politics, not cryptography. But now collusion is *visible*. |

## Recommended pitch framing (for stakeholder decks)

- "The 2026 crisis wasn't a technology problem — it was a **centralised-trust** problem. Blockchain replaces bureaucratic trust with cryptographic trust."
- "Radhakrishnan Committee said: independent audit, digital security, reduce third-party dependence. We deliver all three by construction."
- "Every claim we make is verifiable by any citizen — no need to trust us, or NTA, or the government."
- "Retrofit, not rip-and-replace. Runs alongside NTA/TCS pipeline; incremental adoption."

## Next steps (concrete)

1. **Write threat model doc** (`09-architecture/02-threat-model.md`) — map every attack in `01-problem-analysis` to a specific mitigation in this stack.
2. **Scaffold monorepo** — `contracts/`, `ceremony/`, `wallet/`, `invigilator/`, `verifier/`, `docs/`.
3. **Prototype threshold BLS ceremony** — the single highest-risk technical piece. If this doesn't work, nothing else matters.
4. **Reach out to** SPRIN-D-style incubators / iSPIRT / MoE innovation cell. There is a live political window.

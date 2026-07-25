# Cryptographic Primitives for Secure Exam Distribution

_The blockchain is just a coordinator. The actual leak-prevention lives in these primitives._

## The five primitives that matter

### 1. Threshold cryptography (t-of-n)
- **What:** Private key never fully exists in one place. Split into n shares; any t can jointly decrypt/sign without ever reconstructing the full key.
- **Why for exams:** Question paper is encrypted under a public key whose private key is threshold-split among {NTA, external auditor, judicial rep, state edu board, IIT dean}. Decryption requires t (say 3-of-5) to jointly participate — at T-0, live, on video. No single custodian ever holds the key.
- **Libs:** [threshold-BLS](https://github.com/nikkolasg/blssig), Distributed Key Generation (DKG) from [dfinity/threshold-crypto](https://github.com/dfinity/threshold-crypto), [Shamir crates](https://crates.io/crates/vsss-rs).
- **Real deployments:** DFINITY / Internet Computer randomness, tBTC federation, Chainlink DECO.

### 2. Shamir's Secret Sharing (SSS)
- Simpler cousin of threshold crypto. Split a secret S into n shares such that any t reconstruct it, fewer than t reveal nothing.
- **Why for exams:** Straightforward implementation for the master AES key of the paper.
- **Weakness vs threshold crypto:** requires reconstruction of the raw key (brief window of full-key existence). Threshold crypto avoids even that.
- Use SSS for prototype; upgrade to threshold BLS/ECDSA for prod.

### 3. Timed-release encryption (TRE)
- **What:** Ciphertext that provably cannot be decrypted before time T, even by the encryptor.
- **Two flavours:**
  - **Time-lock puzzles** (Rivest–Shamir–Wagner 1996): decryption requires ~T seconds of sequential computation.
  - **TRE-with-trust-agent:** a beacon/committee publishes decryption keys on a schedule; combined with threshold, no single agent can release early.
- **Why for exams:** Even if the ciphertext leaks weeks before, it's mathematically undecryptable until T-0.
- **Modern refs:** [tlock](https://github.com/drand/tlock) (drand's timelock built on threshold-BLS), Chainlink VRF+time. Also see IACR 2004/231 (Cheon et al.), Springer TR-CSS 2016.

### 4. Verifiable Random Functions (VRFs)
- **What:** Function f(sk, seed) = (value, proof). Anyone can verify the value was correctly derived from a secret key + seed, but only the key-holder could produce it.
- **Why for exams:**
  - **Per-candidate question ordering:** each candidate gets a unique shuffled paper derived from `VRF(nta_sk, candidate_did)` — verifiable, unpredictable, no two candidates get identical papers → drastically reduces value of leaking a "the paper."
  - **Question selection from a bank:** draw N questions from bank of 10N via VRF; the actual questions revealed only at T-0.
- **Prod-grade libs:** Algorand VRF, drand, RFC 9381 (ECVRF).

### 5. Zero-Knowledge Proofs (ZKPs)
- **What:** Prove statement is true without revealing why.
- **Why for exams:**
  - Candidate proves eligibility (age > 17, class 12 pass) without revealing DOB / marks.
  - Centre proves "we verified 3-of-4 VCs for candidate X" without revealing which VCs.
  - Auditor proves "the released paper matches the sealed commitment" without ever seeing the paper.
- **Tooling:** Circom + snarkjs (Groth16, PLONK), Halo2, RISC Zero (general-purpose zkVM), Noir.

## Putting them together — the "paper lifecycle" protocol

```
T-60 days   Question authors write questions in an air-gapped facility.
            Each question committed as SHA-256 hash to blockchain (bank hash).

T-30 days   Distributed Key Generation (DKG) ceremony:
            5 independent authorities generate a threshold public key PK.
            No party ever holds the full private key.

T-14 days   Master paper (or bank) encrypted under PK.
            Ciphertext + Merkle root committed on-chain.
            Ciphertext distributed to all centres via IPFS + regional CDN.
            NO ONE CAN DECRYPT YET.

T-1 hour    Each candidate gets a per-candidate shuffle via VRF(NTA_sk, DID).
            Shuffle commitment published on-chain.

T-0         Threshold ceremony live-streamed: 3-of-5 authorities each submit
            their partial decryption via smart contract. Combined key
            released; ciphertext decryptable at every centre simultaneously.
            Every centre's decryption event logged on-chain.

T+exam      Answer sheets encrypted at candidate device, uploaded to IPFS,
            hash committed to chain. Later grading against known-good key.

T+7 days    Marks issued as W3C VCs to candidate DigiLocker wallet.
```

**Key property:** at every timestep, the *set of parties who could possibly decrypt* is public and cryptographically enforced. A leak before T-0 is provably impossible unless t authorities collude — which is visible.

## The Huszti–Pethő and Giustolisi lineage

Formal academic work on secure exams isn't a footnote — it's the theoretical bedrock. Read at minimum:

- **Huszti & Pethő (2010)** — first formal exam protocol with authentication + candidate anonymisation + question confidentiality even with corrupted authorities. Uses threshold Shamir + timed-release.
- **Giustolisi et al. — Remark! protocol** — auth + conditional anonymity with minimal trust assumptions; formal verification with ProVerif.
- **Dreier, Giustolisi, Kassem, Lafourcade, Lenzini (2015)** — formal verifiability framework for exams; the go-to reference for "what security property does my protocol satisfy?"
- **Giustolisi book, "Modelling and Verification of Secure Exams" (2018)** — full formal treatment in applied pi-calculus.

## Sources

- https://eprint.iacr.org/2004/231.pdf (Timed-Release and Key-Insulated PKE — Cheon et al.)
- https://people.csail.mit.edu/rivest/pubs/RSW96.pdf (Time-lock puzzles — Rivest, Shamir, Wagner)
- https://arxiv.org/pdf/1401.5895 (Timed-Release Secret Sharing Information-Theoretic Security)
- https://link.springer.com/article/10.1007/s10623-016-0324-2 (Timed-release computational secret sharing and threshold encryption)
- https://arxiv.org/pdf/1512.04751 (Design and Analysis of Secure Exam Protocols — Dreier et al.)
- https://hedera.com/wp-content/uploads/2025/11/2021-800.pdf (Incremental Timed-Release Encryption)
- https://www.researchgate.net/publication/272144717_Remark_A_Secure_Protocol_for_Remote_Exams
- https://www.researchgate.net/publication/323862485_The_Huszti-Petho_Protocol
- https://link.springer.com/chapter/10.1007/978-3-031-25734-6_6 (Secure Internet Exams Despite Coercion)
- https://orbilu.uni.lu/bitstream/10993/67137/1/Secure%20and%20Verifiable%20Coercion-Resistant%20Electronic%20Exam_R2.pdf
- https://eprint.iacr.org/2015/1048.pdf (VRFs from Standard Assumptions)
- https://eprint.iacr.org/2025/312.pdf (Traceable VRFs — Boneh, Partap, Rotem 2025)

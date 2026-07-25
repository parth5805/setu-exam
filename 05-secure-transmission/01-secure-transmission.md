# Secure Digital Paper Transmission

_Once we've fixed the "central plaintext" problem with threshold crypto, the transport becomes almost trivial. But there are still design choices that matter._

## The transmission threat model

- Attacker has full read access to the network (they own a courier, a router, a datacenter).
- Attacker may have write access to storage (can copy ciphertext, can't modify without detection).
- Attacker cannot force >= t of the threshold-key holders to sign.
- Attacker may compromise a centre's local machine, but not the biometric-verified invigilator device.

## The three transport tiers

### Tier 1: Bulk ciphertext distribution (T-14 days → T-1 hour)
- Encrypted paper is a **public artefact** — safe to CDN globally, safe to torrent, safe to put on IPFS.
- Content-address via CID; hash on blockchain.
- **No confidentiality requirement here** — the ciphertext is useless without the threshold key.
- Multiple redundant paths: IPFS, government CDN (NIC), DVDs to remote centres (yes, physical is fine because it's ciphertext).

### Tier 2: Threshold key material (T-0)
- Small, high-value.
- Delivered via **partial decryptions submitted to a smart contract** or a dedicated MPC channel (TLS 1.3 + client cert + threshold sig from each authority).
- Every partial is signed by the authority's DID; auditor + public verify.
- No centre ever pulls "a key" — the centre pulls the *combined threshold signature* the moment 3-of-5 authorities have submitted.

### Tier 3: Per-candidate paper delivery inside the centre (T-0 onwards)
- Encrypted paper decrypted on the invigilator/proctor server (once combined key is available).
- Per-candidate variant computed via VRF; delivered to each candidate machine over centre LAN with:
  - **TLS 1.3 mutual auth** (each candidate device provisioned with cert bound to seat DID).
  - **Ephemeral session key per candidate** — no reuse.
  - Content **never touches disk in plaintext** on the candidate machine (in-memory only, kept in an SGX/TrustZone enclave if available).

## Standard building blocks

| Property | Primitive | Notes |
|---|---|---|
| Confidentiality in transit | TLS 1.3 with X25519, AES-GCM-256 | Standard, works everywhere |
| Confidentiality at rest | AES-256-GCM with threshold-derived DEK | DEK never in one place |
| Authenticity | Ed25519 signatures from authority DIDs | Small, fast, mature |
| Integrity | SHA-256 Merkle root on chain | Enables partial verification |
| Freshness / anti-replay | Timestamps + nonces + block heights | Prevent old-key replay |
| PFS | Ephemeral ECDH per session | So a future key leak doesn't retro-decrypt |
| Metadata protection | Onion routing / Tor for centre reports | Attacker can't map "which centre asked for what" |

## Existing patents / prior art (informative)

- **US 11,062,023 — Secure distribution and administration of digital examinations.** Describes centre-level decryption gated on multi-party approval.
- **US 9,135,671 — Secured computer based assessment.** Local secure browser + centralised key release.
- **US 11,521,507 — Method and system for securely conducting a digital examination.** Cryptographic session per candidate, no reuse.

## Real-world reference points

- **TCS iON (JEE Main / NEET-UG CBT):** "Question paper in encrypted format reaches every exam centre and is decrypted only when the first candidate clicks the question paper." Good pattern — but decryption authority is TCS/NTA alone (single-party). Threshold split would harden it.
- **Safe Exam Browser (SEB):** open-source lockdown browser used globally. Ships with kiosk mode, disables screenshots, disables clipboard. Free, MIT-licensed, worth integrating.
- **BlinkExam / ExamSoft:** commercial. 256-bit TLS + 2FA + SEB pattern.

## What "secure digital transmission" should mean in the new NTA spec

A recommendation-grade spec:

1. Paper never exists in plaintext outside a decryption session inside a centre.
2. Decryption session requires (a) live threshold from independent authorities, (b) biometric-verified invigilator, (c) time-window match, all logged on-chain.
3. All transport is over TLS 1.3 with mutual auth using DIDs.
4. All artefacts are content-addressed (IPFS-style) and Merkle-committed on-chain.
5. No SSH into centre boxes during exam window; only smart-contract-mediated commands.
6. Centre machines are locked-down kiosks (SEB or equivalent) — no USB, no camera, no network beyond the exam VPN.

## Sources

- https://www.researchgate.net/publication/4238793_A_secure_e-exam_management_system
- https://www.academia.edu/105966831/Enhanced_Digital_Examination_Assessment_System_with_Secured_Access_Using_Cryptographic_Techniques
- https://www.researchgate.net/publication/283690305_A_Secure_Exam_Protocol_Without_Trusted_Parties
- https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/11062023 (US patent 11062023)
- https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/9135671 (US patent 9135671)
- https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/11521507 (US patent 11521507)
- https://ieeexplore.ieee.org/document/1625398/ (Secure e-exam management system — IEEE)
- https://blinkexam.com/blog/building-trust-in-digital-exams-security-and-proctoring-in-online-assessment-software/
- https://www.researchgate.net/publication/228391195_A_secure_electronic_exam_system
- https://tcsionblog.wordpress.com/2019/08/14/jee-main-2019-how-does-nta-secure-the-exam/
- https://safeexambrowser.org/

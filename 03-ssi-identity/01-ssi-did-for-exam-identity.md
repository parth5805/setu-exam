# Self-Sovereign Identity for Exam Candidates

## The problem SSI solves in exams

Impersonation is currently defeated (badly) by biometric checks against Aadhaar records. As MP 2023 showed, **the enrolment authority itself can be corrupted** — solver gangs paid Aadhaar operators to swap biometric records so a solver's face/fingerprint matched the candidate's record.

If the ground truth (Aadhaar record) can be tampered with, the entire biometric verification chain collapses.

SSI/DID reframes identity so that:
- The candidate **holds** their credentials in their own wallet.
- Credentials are **cryptographically signed** by multiple independent issuers (school board, coaching institute, previous exam, ID authority).
- Verification happens **against the signatures**, not against a single central DB that can be silently mutated.
- Any credential swap requires forging **multiple issuer signatures**, not bribing one operator.

## Core primitives

### W3C DIDs (Decentralized Identifiers)
- Globally-unique, self-owned identifier: `did:key:z6Mk...`, `did:web:nta.ac.in`, `did:ion:...`.
- Resolves to a DID document containing public keys + service endpoints.
- No central registrar can silently reassign — a `did:web:` change is a public git commit; `did:ion:` is anchored on Bitcoin; `did:polygon:` on Polygon.

### W3C Verifiable Credentials (VCs)
- Tamper-evident, signed JSON-LD or JWT.
- Example: `NTA certifies that candidate did:key:xyz enrolled in NEET-UG 2027 with application no. 12345, tied to face template hash 0xabc...`.
- Selective disclosure via **BBS+ signatures** → prove you're eligible without revealing name/DOB.
- Zero-knowledge proofs → prove age > 17 without revealing DOB.

### Wallets
- Candidate wallet holds VCs, presents them at exam centre via QR/NFC.
- Verifier (invigilator app) checks issuer signature offline.

## Concrete design for Indian exams

### Candidate onboarding (T-90 days)
1. Candidate applies via NTA portal.
2. NTA issues **Candidate VC** signed with `did:web:nta.ac.in`, containing:
   - Application ID
   - Hash of biometric template
   - Hash of photo
   - Eligibility claims (class 12 pass, etc.)
3. Candidate's school issues **Educational VC** (independent second attestation).
4. UIDAI issues **Aadhaar-linked VC** (third attestation, using existing Aadhaar Face API).
5. Optional: Coaching institute issues attendance VC (fourth).
6. All VCs stored in candidate's DigiLocker wallet (already trusted, already deployed).

### Anti-impersonation at exam centre (T-0)
1. Candidate scans QR at entry.
2. Invigilator device requests **presentation of 3-of-4 VCs** + live face capture.
3. Local ZK verification against all issuer public keys (no internet dependency).
4. Live face template compared to signed hash inside VC (not to any central DB — so no "tamper Aadhaar record" attack).
5. Every check produces a signed attestation → written to blockchain as an anonymous "candidate X verified at centre Y at time T" event.

### Why this beats current Aadhaar-only

- **Multi-issuer:** attacker must corrupt UIDAI + school + NTA simultaneously.
- **Immutable at issuance:** VC signatures are frozen at T-90; no silent later mutation.
- **Offline verifiable:** works even if the centre loses internet.
- **Auditable:** anonymous verification events on-chain — impossible to quietly skip a check.

## India-specific integration path

- **DigiLocker as wallet.** ~400M+ Indians already have it. Government departments already issue "verifiable" credentials into it.
- **W3C VC envelope over DigiLocker XML.** Existing DigiLocker docs wrapped in W3C VC format = interoperable with international SSI stack. Gradify Labs and Protean have implementations.
- **National Academic Depository (NAD)** integration for the educational VC.
- **UIDAI Face Authentication API** (proven in NEET-UG 2025 PoC) — but consumed as *one signal among many*, not the sole gate.

## Frameworks / libs to build with

- **Hyperledger Aries / Indy** — the reference SSI stack, mature Python/JS/Rust libs.
- **Veramo** — JS/TS DID+VC framework, fast prototyping.
- **Spruce ID (DIDKit)** — Rust core, works with `did:key`, `did:ethr`, `did:pkh`.
- **Trinsic / Dock** — SaaS-ish but real production users.
- **BBS+ signatures** for selective disclosure — MATTR, Hyperledger AnonCreds v2.

## Sources

- https://liberlion.medium.com/self-sovereign-identity-ssi-ba22c2c91583
- https://www.dock.io/post/self-sovereign-identity
- https://www.okta.com/identity-101/self-sovereign-identity/
- https://101blockchains.com/self-sovereign-identity-and-decentralized-identity/
- https://arxiv.org/pdf/2508.01913 (Decentralized Framework for Ethical Authorship Validation via SSI)
- https://arxiv.org/pdf/1906.09791 (Blockchain-based digital identity verification, record attestation)
- https://arxiv.org/pdf/2406.00620 (Model-Driven Security Analysis of SSI Systems)
- https://www.gradifytech.com/verifiable-credentials-india (VCs in India — W3C VC, DID, NAD, ABC)
- https://www.proteantech.in/articles/certificate-issuance-digital-identity/
- https://www.truscholar.io/blog/digilocker-vs-blockchain-based-credentials-a-comparative-analysis
- https://www.biometricupdate.com/202407/indias-upsc-seeks-biometric-authentication-tech-to-combat-civil-service-exam-fraud
- https://www.newsonair.gov.in/uiadi-successfully-conducted-face-authentication-during-neet-ug-2025

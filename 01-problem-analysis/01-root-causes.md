# Root Causes of Paper Leaks in India

_Any technical solution must attack these specific failure modes. If your system doesn't kill one of these, it's not solving the real problem._

## 1. Structural / Design Vulnerabilities

- **Single-day, single-shot, single national paper.** NEET-UG runs one common paper for ~2.3M candidates. If one packet leaks pre-bell, every desk in the country has the same paper.
- **Pen-and-paper format.** Requires physical printing, packing, courier, storage — every hop is a leak vector.
- **Centralised secret with wide distribution.** The same secret sits in thousands of places for hours before use.

## 2. Printing Press & Logistics Chain

- Question papers are **printed centrally**, physically packed, transported through layers of custodians to thousands of schools, and stored overnight.
- Printing is **outsourced to multiple vendors**; investigators found courier staff with unauthorised access to sealed packets.
- **A 30-minute lapse is enough** to digitise (photograph) a paper and leak it via WhatsApp.
- Exam papers **lack the surveillance depth** applied to currency printing.

## 3. Standard Operating Procedure (SOP) Failures

- Tampering with NTA's SOPs for transportation, storage, hand-over and take-over.
- SOPs **not followed at examination centres**.
- Custody-chain gaps under CBI radar: printing press → transport → district hub → centre-level custodian.

## 4. Center-Level & Third-Party Risk

- NTA depends heavily on **private examination centre operators and third-party logistics providers**.
- Radhakrishnan Committee explicitly flagged this: recommended **expanding directly-operated government centres** and reducing dependence on private ones.

## 5. Human / Insider Threat

- Solver gangs (~₹4–15 lakh per candidate) bribe printing insiders, invigilators, biometric contractors.
- **Impersonation is a separate massive vector** (see below) — even a leak-proof paper is worthless if someone else sits the exam.

## 6. Biometric / Impersonation Vector

- **MP 2023 constable exam:** Aadhaar operators modified biometric records so hired "solvers" could sit in place of candidates. Aadhaar centres in Bhitarwar, Morena, Sheopur implicated.
- **NEET re-exam:** Employees of the biometric-verification contractor themselves were arrested. Bihar Police arrested 24 in Lakhisarai solver-gang network.
- The impersonation vector proves that **any identity layer built on top of Aadhaar must assume the Aadhaar record itself can be tampered** at enrolment.

## 7. Institutional / Governance Failures

- NTA is heavily **contractual-staff dependent**, not permanent — flagged as a security risk.
- No independent auditor of exam ops in real time.
- Post-incident forensics are slow and rely on manual whistleblowers.

## Attack-surface summary → design targets

| Vector | What the technical solution must guarantee |
|---|---|
| Central printing insider | No plaintext paper exists outside cryptographic seal until T-0 |
| Courier / storage leak | Ciphertext is useless before T-0; possession without decryption keys ≠ compromise |
| Center-level operator | Center cannot decrypt papers alone; needs live threshold from independent parties |
| Impersonation at centre | Biometric bind to candidate cannot be forged at enrolment; live liveness + independent verifier |
| SOP tampering | Every custody-chain event is signed, timestamped, publicly auditable — no "quiet" access |
| Post-hoc forensics | Immutable timeline of every access, encryption, decryption event |
| Question predictability | Per-candidate shuffle from a verifiable random beacon — no single "the paper" to leak |

## Sources

- https://www.printweek.in/article/neet-ug-2026-leak-exposes-printing-press-weak-links/4saespfvmeksgrg2yr3jn94fdy
- https://www.scconline.com/blog/post/2026/05/15/neet-2026-paper-leak-examination-incident-explained/
- https://theprobe.in/education/how-neet-let-the-paper-leak-2026-11854573
- https://openthemagazine.com/india/a-neet-solution-needs-treating-the-root-causes
- https://www.visionias.in/blog/current-affairs/neet-ug-2026-paper-leak-failure-of-indias-exam-ecosystem
- https://en.wikipedia.org/wiki/Paper_leak_in_India
- https://www.tribuneindia.com/news/india/neet-ug-probe-printing-press-to-exam-centres-paper-custody-chain-under-cbi-radar-633758
- https://educationpost.in/news/national/solver-gang-fakes-aadhaar-to-hijack-police-constable-exam-in-madhya-pradesh
- https://idtechwire.com/biometric-contractor-staff-among-those-arrested-in-alleged-indian-exam-impersonation-racket/
- https://the420.in/mp-police-exam-fraud-aadhaar-biometrics-scam-2023/

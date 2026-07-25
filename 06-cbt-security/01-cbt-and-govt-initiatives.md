# CBT Security & Existing Indian Govt Initiatives

## Current CBT security stack (TCS iON / NTA — JEE Main, etc.)

TCS iON is the incumbent for most large-scale Indian CBT (JEE Main, GATE, CAT partly, many PSU). What they do today:

- **Encrypted paper delivery to centres.** Ciphertext at each centre in advance; decrypted "only when the first candidate clicks" — so no plaintext exists in advance at the centre.
- **Multi-dimensional randomisation.** Randomises exam centre allocation, candidate lab seating, question order per candidate, option order per candidate, staff allocation.
- **Per-candidate unique paper.** Same question bank, different ordering per candidate — reduces leak value.
- **Biometric + facial impersonation detection** at check-in.
- **Remote proctoring option:** camera feed to central AI/ML server, real-time anomaly detection.
- **Secure browser lockdown.**

### Where this is still weak (attack surface for us to close)

1. **Single-party decryption authority.** TCS/NTA holds the decryption key. If TCS is compromised (or forced), the key leaks.
2. **Centralised biometric DB.** Aadhaar/NTA record is the trust anchor — as MP 2023 showed, it's mutable.
3. **No public auditability.** Only NTA/TCS see the logs. Post-CJP, this is politically untenable.
4. **Third-party centre operators** still control the physical environment.
5. **No cryptographic proof that "the paper delivered = the paper committed weeks ago."** Trust in NTA is implicit.

## Public Examinations (Prevention of Unfair Means) Act, 2024

Passed 2024-02, in force 2024-06-21. Scope: NTA, UPSC, SSC, RRB, banking exams.

- Any person using unfair means: **3–5 years jail + up to ₹10 lakh fine**.
- Organised crime (solver gangs): **5–10 years + at least ₹1 crore fine**.
- **Bona fide candidates exempt** — targets facilitators, not students.
- Establishes formal SOPs the technical solution can bind to.

Reference: https://en.wikipedia.org/wiki/Public_Examinations_(Prevention_of_Unfair_Means)_Act,_2024

## Radhakrishnan Committee (High-Level Committee on NTA Reforms), 2024

Chaired by Dr. K. Radhakrishnan (former ISRO chair). Constituted 2024-06-22 after NEET 2024. Recommendations directly relevant:

1. Move entrance exams **online (CBT)** where possible.
2. **Hybrid model** where CBT infeasible.
3. **Restructure NTA operations** — more permanent staff, less contractual.
4. **Expand directly-operated govt exam centres**, reduce private/third-party dependence.
5. **Digital security upgrades** for NEET, CUET (explicitly named).
6. **Independent audit** of exam processes.

This is the govt's own roadmap. **Any prototype should map explicitly onto these recommendations** — makes it politically buyable.

## The Indian identity/document stack we can plug into

| Component | What it is | How to use in exam system |
|---|---|---|
| **Aadhaar** | 1.3B enrolled, biometric-backed | Face API for liveness (not sole trust) |
| **Aadhaar Face Authentication API** | UIDAI service, live face check | Piloted NEET-UG 2025, worked |
| **DigiLocker** | ~400M users, govt-issued docs | Wallet for VCs (candidate credential store) |
| **National Academic Depository (NAD)** | Govt-signed edu certs | Educational VC issuer |
| **eSign** | Aadhaar-based signature service | Candidate signs consent, exam attempt |
| **Academic Bank of Credits (ABC)** | Credit registry | Post-exam credit issuance |
| **DIKSHA** | MoE platform | Not for exams, but ecosystem context |

W3C VC wrapping over these is being pushed by **Gradify Labs, Protean, Truscholar**. Working code exists.

## The politically-loaded landscape (know your audience)

- **NTA** is on the defensive — will accept anything with public verifiability + independent audit story.
- **MoE** (post-Pradhan) needs a visible technology-forward win in Q3/Q4 2026.
- **CJP + student unions** want auditability *by them*, not just by government.
- **UIDAI** has shown willingness (NEET-2025 face auth PoC) to integrate.
- **State education boards** are the messy layer — each state runs its own exams. A federated design plays well.
- **Judiciary** (Supreme Court oversight of NEET) will look favourably at cryptographic guarantees over bureaucratic assurances.

**Design implication:** the trust story must include the government's critics as first-class verifiers. A permissioned chain with only NTA nodes is dead on arrival.

## Sources

- https://tcsionblog.wordpress.com/2019/08/14/jee-main-2019-how-does-nta-secure-the-exam/
- https://tcsionblog.wordpress.com/2019/04/19/no-human-can-help-you-hack-in-a-computer-based-exam/
- https://www.tcsion.com/international/offerings/assessments/
- https://www.tcs.com/who-we-are/newsroom/press-release/tcs-ion-launches-remote-assessments-product
- https://www.vensysco.com/services/examination
- http://www.nta.ac.in/Download/Tender/TenderTitle_20201116105601.pdf
- https://www.indiacode.nic.in/handle/123456789/20100
- https://en.wikipedia.org/wiki/Public_Examinations_(Prevention_of_Unfair_Means)_Act,_2024
- https://prsindia.org/billtrack/the-public-examinations-prevention-of-unfair-means-bill-2024
- https://organiser.org/2024/10/30/263052/bharat/radhakrishnan-committee-proposes-digital-security-upgrades-for-neet-cuet-exam-reforms-following-neet-paper-leak/
- https://pwonlyias.com/editorial-analysis/revamping-national-testing-agency/
- https://innovateindia.mygov.in/examination-reforms-nta/
- https://x.com/EduMinOfIndia/status/1806594166362689965
- https://www.digilocker.gov.in/
- https://en.wikipedia.org/wiki/DigiLocker
- https://www.gradifytech.com/verifiable-credentials-india
- https://www.proteantech.in/articles/certificate-issuance-digital-identity/

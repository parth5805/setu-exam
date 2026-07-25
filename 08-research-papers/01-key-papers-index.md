# Research Papers Index

_Curated set to read before writing code. Each entry: what to take from it, and how it plugs into our system._

## A. Formal e-exam protocols (foundational)

### 1. Huszti & Pethő, "The Huszti-Pethő Protocol" (2010)
- **URL:** https://www.researchgate.net/publication/323862485_The_Huszti-Petho_Protocol
- **Take:** First formal exam protocol handling authentication + candidate anonymity + question confidentiality **even with corrupted authorities**. Uses threshold Shamir + timed-release service.
- **Use for:** Baseline protocol our design must strictly dominate.

### 2. Dreier, Giustolisi, Kassem, Lafourcade, Lenzini, "Design and Analysis of Secure Exam Protocols" (2015)
- **URL:** https://arxiv.org/pdf/1512.04751
- **Take:** Formal framework for exam verifiability, applicable to paper + e-exams. Provides the vocabulary ("authentication," "question integrity," "mark integrity," "test-answer authentication," etc.).
- **Use for:** Property list our smart contracts must enforce.

### 3. Giustolisi et al., "Remark! A Secure Protocol for Remote Exams" (2014)
- **URL:** https://www.researchgate.net/publication/272144717_Remark_A_Secure_Protocol_for_Remote_Exams
- **Take:** Remote exam with conditional anonymity, minimal trust. Analysed with ProVerif.
- **Use for:** Reference for the remote-proctoring flow.

### 4. Giustolisi, "Modelling and Verification of Secure Exams" (Springer 2018)
- **URL:** https://www.fmeurope.org/2021/10/29/book-review-modelling-and-verification-of-secure-exams/
- **Take:** Book-length treatment. Applied π-calculus models for exam protocols; formal verification recipes.
- **Use for:** Cite in any RFP response — this is the field's textbook.

### 5. "Secure and Verifiable Coercion-Resistant Electronic Exam" (2024)
- **URL:** https://orbilu.uni.lu/bitstream/10993/67137/1/Secure%20and%20Verifiable%20Coercion-Resistant%20Electronic%20Exam_R2.pdf
- **Take:** Adds coercion-resistance to Giustolisi lineage (relevant if candidates are pressured to leak or misbehave).

### 6. "Auditable Anonymous Electronic Examination" (2024)
- **URL:** https://www.researchgate.net/publication/380284859_Auditable_Anonymous_Electronic_Examination
- **Take:** Combines auditability + anonymity (grader can't identify candidate; auditor can verify process).

## B. Blockchain-for-exams (application literature; quality varies)

### 7. "SafePaper: Blockchain-Based Secure Examination Paper Storage with Shamir's Secret Sharing"
- **URL:** https://www.academia.edu/143141420/SafePaper_Blockchain_Based_Secure_Examination_Paper_Storage_with_Shamirs_Secret_Sharing
- **Take:** Direct match for our design pattern. Extract the SSS key-splitting model.

### 8. "Paper Leakage Prevention Using Blockchain" — IJIRMPS 2026
- **URL:** https://www.ijirmps.org/papers/2026/1/232926.pdf
- **Take:** Recent (post-2026-leak) attempt. Assess implementation choices; note gaps.

### 9. "Question Paper Prevention System Using Blockchain Technology" — JETIR
- **URL:** https://www.jetir.org/view?paper=JETIR2507308
- **Take:** Smart-contract time-locked distribution pattern.

### 10. "Blockchain Framework for Securing and Distributing Exam Papers" — IJSREM
- **URL:** https://ijsrem.com/download/blockchain-framework-for-securing-and-distributing-exam-papers/
- **Take:** IPFS + smart contract architecture reference.

### 11. "Blockchain-based Competitive Examination System in India" — IJIRSET Aug 2024
- **URL:** https://www.ijirset.com/upload/2024/august/80_Blockchain.pdf
- **Take:** Explicitly Indian context — good baseline of what's been proposed here.

### 12. "Question Paper Leakage Prevention using Blockchain" — IJMRSET
- **URL:** https://www.ijmrset.com/upload/101_Question%20Paper.pdf
- **Take:** Alternate implementation pattern.

## C. Cryptographic primitives (foundational)

### 13. Rivest, Shamir, Wagner, "Time-lock Puzzles and Timed-release Crypto" (MIT 1996)
- **URL:** https://people.csail.mit.edu/rivest/pubs/RSW96.pdf
- **Take:** The original TRE paper. Read it once. The RSW time-lock is still the baseline.

### 14. Cheon et al., "Timed-Release and Key-Insulated Public Key Encryption" (IACR ePrint 2004/231)
- **URL:** https://eprint.iacr.org/2004/231.pdf
- **Take:** Proves equivalence of TR-PKE and strongly key-insulated encryption; grounding for choosing scheme.

### 15. "Timed-release computational secret sharing and threshold encryption" (Springer 2016)
- **URL:** https://link.springer.com/article/10.1007/s10623-016-0324-2
- **Take:** TR-CSS scheme — participants reconstruct only after time T.

### 16. "Timed-Release Secret Sharing with Information-Theoretic Security" (2014)
- **URL:** https://arxiv.org/pdf/1401.5895
- **Take:** Information-theoretic TR-SS — strongest security assumption.

### 17. Boneh, Partap, Rotem, "Traceable Verifiable Random Functions" (IACR 2025/312)
- **URL:** https://eprint.iacr.org/2025/312.pdf
- **Take:** Latest VRF construction with traceability. Directly applicable to per-candidate question ordering.

### 18. "Verifiable Random Functions from Standard Assumptions" (IACR 2015/1048)
- **URL:** https://eprint.iacr.org/2015/1048.pdf
- **Take:** VRF fundamentals.

## D. Self-sovereign identity / DID / VC

### 19. "Towards a Blockchain-based digital identity verification, record attestation and record sharing system" (arXiv 1906.09791)
- **URL:** https://arxiv.org/pdf/1906.09791
- **Take:** DID + VC architecture for identity + record attestation — direct fit for candidate onboarding.

### 20. "Model-Driven Security Analysis of Self-Sovereign Identity Systems" (arXiv 2406.00620)
- **URL:** https://arxiv.org/pdf/2406.00620
- **Take:** Threat modelling for SSI stacks. Read before designing wallet flow.

### 21. "Decentralized Framework for Ethical Authorship Validation" (arXiv 2508.01913)
- **URL:** https://arxiv.org/pdf/2508.01913
- **Take:** Adjacent domain (academic authorship) using SSI + blockchain — patterns transfer.

## E. IPFS + access control (storage layer)

### 22. "Blockchain-Based, Decentralized Access Control for IPFS" (2018)
- **URL:** https://www.researchgate.net/publication/327034734_Blockchain-Based_Decentralized_Access_Control_for_IPFS
- **Take:** Reference pattern for gated IPFS reads via smart contract.

### 23. "Secure File Sharing Using Blockchain and IPFS with Smart Contract-Based Access Control" (2024)
- **URL:** https://www.researchgate.net/publication/389825946_Secure_File_Sharing_Using_Blockchain_and_IPFS_with_Smart_Contract-Based_Access_Control
- **Take:** Recent, close to our transmission tier.

## F. Indian regulatory / policy documents

### 24. Public Examinations (Prevention of Unfair Means) Act, 2024
- **URL:** https://www.indiacode.nic.in/handle/123456789/20100
- **Take:** Legal frame. Our system's logs must be admissible evidence under this Act.

### 25. Radhakrishnan Committee reforms (2024)
- **URL:** https://organiser.org/2024/10/30/263052/bharat/radhakrishnan-committee-proposes-digital-security-upgrades-for-neet-cuet-exam-reforms-following-neet-paper-leak/
- **Take:** Government's own reform roadmap — align pitch to these bullet points.

## Reading order (if time-constrained)

1. Root causes doc (01-problem-analysis) — internal.
2. Dreier et al. 2015 (paper #2) — vocabulary.
3. Huszti-Pethő (paper #1) — baseline protocol.
4. Rivest-Shamir-Wagner 1996 (paper #13) — TRE intuition.
5. SafePaper (paper #7) — closest to our target.
6. Radhakrishnan recs (paper #25) — political frame.
7. Giustolisi book (paper #4) — deep dive when writing formal spec.

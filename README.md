# SetuExam — An Open Letter to India

> **सेतु** _(setu)_ = bridge. A bridge between the exam system we have and the one we deserve.

**To:** Every student who woke up on 12 May 2026 to find their year erased. Every parent who mortgaged land to pay for coaching. Every family that buried a child over a rank. The National Testing Agency. The Ministry of Education. The Supreme Court of India. The developer community of Bharat.

**From:** A blockchain developer, and anyone who joins after this letter.

**Date:** 25 July 2026 — the day Union Education Minister Dharmendra Pradhan resigned.

---

## Why this letter exists

On 3 May 2026, 2.27 million young Indians sat NEET-UG. Nine days later, the exam was cancelled — the paper had leaked from a Nashik printing press before the seals were even broken. A whistleblower matched 45 questions from a leaked PDF; teachers later matched 90 more.

The 2024 leak. The 2026 leak. Every state PSC leak in between. **These are not accidents. They are the predictable output of a system that trusts 120,000+ humans to keep a secret across 15 stages over 90 days.**

The Cockroach Janta Party protests on Delhi's Jantar Mantar since 6 June 2026 have already forced the resignation of a Union Minister. That is a political win. It is not yet a fix.

**The fix is technical, and it exists. We are going to build it, in the open, together.**

---

## What we're building

**SetuExam** is a blockchain-native, cryptographically-verifiable examination pipeline. Two ideas do the load-bearing work:

1. **Threshold encryption.** The question paper is encrypted under a key that is mathematically split across **five independent authorities** — NTA, an independent auditor, a judicial observer, a state board, and a civil-society verifier. **No single party can decrypt.** The key is only reassembled in a live, publicly-streamed ceremony at T-0 on exam day. Even if the ciphertext leaks a month early, it is undecryptable garbage.

2. **Multi-issuer identity.** Every candidate carries a bundle of W3C Verifiable Credentials in their DigiLocker — signed by NTA, their school, UIDAI, and their coaching institute. Impersonation now requires corrupting *four* independent institutions simultaneously, not one bribed Aadhaar operator.

Add per-candidate paper shuffling (via verifiable random functions), on-chain custody audit, hardware-attested grading — and you get an exam system where **the leak vectors of the current system are mathematically impossible, not policy-forbidden.**

Full technical design: [`09-architecture/01-proposed-architecture.md`](09-architecture/01-proposed-architecture.md)

---

## What's in this repository (day 1)

This is the **research phase**, complete and public. Nothing here is a secret; everything cites public sources.

| Folder | What's inside |
|---|---|
| [00-context](00-context/) | The 2026 crisis timeline, verified from public news |
| [10-current-lifecycle](10-current-lifecycle/) | **How NEET-UG actually works today — 15 stages, ASCII diagrams, 47 leak vectors** |
| [01-problem-analysis](01-problem-analysis/) | Root-cause analysis of paper leaks |
| [02-blockchain](02-blockchain/) | What blockchain solves — and honestly, what it doesn't |
| [03-ssi-identity](03-ssi-identity/) | Self-Sovereign Identity for candidates (W3C DIDs + VCs + DigiLocker) |
| [04-cryptography](04-cryptography/) | Threshold cryptography, timed-release encryption, VRFs, ZK — the primitives |
| [05-secure-transmission](05-secure-transmission/) | Transport tiers, TLS, IPFS, prior art |
| [06-cbt-security](06-cbt-security/) | Current TCS iON/NTA stack + India's regulatory landscape |
| [08-research-papers](08-research-papers/) | 25 curated academic papers you should read |
| [09-architecture](09-architecture/) | **The SetuExam prototype spec** |

## Where to start reading

1. [00-context/00-current-crisis.md](00-context/00-current-crisis.md) — the crisis that triggered this
2. [10-current-lifecycle/01-neet-ug-lifecycle-a-to-z.md](10-current-lifecycle/01-neet-ug-lifecycle-a-to-z.md) — how NEET actually works, and every place it breaks
3. [09-architecture/01-proposed-architecture.md](09-architecture/01-proposed-architecture.md) — how we fix it

---

## How to contribute

This is a public, MIT + CC-BY-4.0 project. If you have any of these, we need you:

- **Cryptographers** — threshold BLS, DKG, timed-release, VRFs, ZK. The math has to be right.
- **Blockchain developers** — Solidity on Polygon, Rust for services, Go for the ceremony daemon.
- **SSI / identity engineers** — Veramo, Hyperledger Aries, DID/VC tooling, DigiLocker integration.
- **Security researchers** — red-team the architecture. Find the holes. That's the only way it becomes real.
- **Educators, invigilators, NTA/state-board insiders** — you know things about the current pipeline no external researcher can. Contribute anonymously via GitHub Issues if you must.
- **Legal / policy folks** — the Public Examinations (Prevention of Unfair Means) Act, 2024 and the Radhakrishnan Committee recommendations are the political frame. Help us map every technical control to a legal/policy hook.
- **Writers / translators** — this letter should exist in every Indian language. All docs should.
- **Designers** — turn ASCII diagrams into real ones. Turn the architecture into a stakeholder deck.
- **Students** — your voice is why this matters. File issues. Ask questions. Poke holes.

### Concrete first tasks (open, waiting)

- Prototype the **threshold BLS ceremony** (Go/Rust). Single biggest technical de-risk.
- Draft `contracts/ExamRegistry.sol`, `contracts/ThresholdCeremony.sol`, `contracts/CandidateRegistry.sol`.
- Wrap **DigiLocker XML** in a W3C VC envelope for the candidate wallet.
- Reference implementation of **per-candidate VRF shuffle**.
- Translate this README into Hindi, Tamil, Bengali, Marathi, Telugu, Gujarati, Kannada, Malayalam, Odia, Punjabi, Assamese, Urdu.
- Convert the [10-current-lifecycle](10-current-lifecycle/) ASCII diagrams into proper illustrations (SVG / Excalidraw).

**Open a GitHub Issue with your name (or pseudonym) and the task you want to take.** No process theatre — pick something, do it, PR it.

Read [CONTRIBUTING.md](CONTRIBUTING.md) before your first PR.

---

## What this project is not

- **Not** a startup pitch. There is no equity. There will never be a token.
- **Not** an attack on NTA, MoE, or the Government of India. It is a gift *to* them — a working prototype and a public design their own committees can adopt.
- **Not** a promise that technology alone fixes exam fraud. Cameras, invigilators, and legal enforcement still matter. This project handles the parts cryptography *can* handle — and it's more than most people realise.
- **Not** finished. It is day one.

## Licenses

- **Code** — [MIT](LICENSE-CODE)
- **Documentation** — [CC BY 4.0](LICENSE-DOCS)

Use it, fork it, sell it, print it in a textbook, take it to Parliament. All we ask is attribution.

## Contact

- **Issues / PRs** — GitHub, this repo. Preferred.
- **Anonymous tips from inside the exam pipeline** — file a GitHub Issue from a throwaway account. We will treat them seriously and never require your identity.

---

## A closing note

The reason the 2024 leak repeated in 2026 is that between them, we changed procedures but not the fundamental architecture of trust. If we do the same again, the 2028 leak is already written.

The Cockroach Janta Party got a minister to resign. That was politics doing its job. Now technology has to do its job.

**If you can help, you already know. Fork the repo. Pick an issue. Ship code.**

**हम कर सकते हैं.** _We can do this._

— The SetuExam contributors

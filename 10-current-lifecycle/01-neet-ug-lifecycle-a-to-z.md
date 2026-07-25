# NEET-UG Paper Lifecycle — A to Z

_The exact path a NEET-UG question paper takes from a professor's mind to a candidate's desk, and every point where it can (and does) leak._

> Every leak vector marked ⚠️ has been used in a real case (mostly NEET-UG 2024 and 2026). Numbered like `L1, L2...` and mapped to defences in [`09-architecture/01-proposed-architecture.md`](../09-architecture/01-proposed-architecture.md).

---

## The full pipeline at a glance

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                     │
│  STAGE 1  →  STAGE 2  →  STAGE 3  →  STAGE 4  →  STAGE 5  →         │
│  Authoring   Moderate    Translate   Master     Print               │
│                                                                     │
│  →  STAGE 6  →  STAGE 7  →  STAGE 8  →  STAGE 9  →  STAGE 10        │
│     Pack        Transport   Bank        Centre     Exam Hall        │
│                 (Press→Hub) Strongroom  (T-day)    (Break seal)     │
│                                                                     │
│  →  STAGE 11  →  STAGE 12  →  STAGE 13  →  STAGE 14                 │
│     Sit exam    Collect     Transport    Scan &                     │
│                 OMR         OMR back     Result                     │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

Legend:
- 🧠 = happens in a human head / private computer
- 📄 = paper artefact exists
- 🚚 = physical transport
- 🔒 = intended-to-be-secure zone
- ⚠️ = known leak point

---

## STAGE 1 — Question Authoring 🧠

**Who:** Subject Matter Experts (SMEs) — professors from IITs, AIIMS, top universities. Recruited by NTA under NDA.

**How:**
- Each SME contributes a broad pool of candidate questions across their subject (Physics / Chemistry / Biology).
- Contribution done individually, sometimes from their own institution/office computers.
- Even the SME does not know *which* of their questions will make the final paper (a deliberate confidentiality mechanism).

```
   [SME Prof A]              [SME Prof B]              [SME Prof C]
     │                          │                          │
     │  writes 40 Q's           │  writes 50 Q's           │  writes 35 Q's
     ▼                          ▼                          ▼
   ┌──────────────────────────────────────────────────────────────┐
   │             Question Bank (large pool)                       │
   │             ~10x the size of the actual paper                │
   └──────────────────────────────────────────────────────────────┘
```

### ⚠️ Leak points at Stage 1

- **L1 — SME device compromise.** SME's laptop is not air-gapped. A keylogger / spyware / physical theft leaks the draft bank. No detection.
- **L2 — SME direct leak.** SME sells or shares questions with a coaching cartel. Because the SME doesn't know which Q's are picked, only *some* of their questions leak — enough for a "guess paper" (this matches the 2026 "90 Biology Q's matched" pattern).
- **L3 — Email / cloud transit.** Files shared over Gmail/WhatsApp/institutional email. No E2E encryption assumed.

---

## STAGE 2 — Moderation (Paper Setting Committee) 🧠🔒

**Who:** A small dedicated moderation panel (per subject), often locked in a facility for the duration.

**How:**
- Panel curates the final N questions from the bank.
- Reviews for correctness, difficulty balance, syllabus fit, ambiguity.
- Produces the master English paper.

Post-2024, NTA reportedly put **paper setters in lockdown** with 5-lakh security personnel deployed.

```
   Question Bank (10N Q's)
          │
          ▼
   ┌──────────────────────────────────┐
   │  Moderation Committee (locked)   │
   │  - Curate                        │
   │  - Balance                       │
   │  - Finalise language & options   │
   └──────────────────────────────────┘
          │
          ▼
   Master Paper (English, 180 Q's for NEET-UG)
```

### ⚠️ Leak points at Stage 2

- **L4 — Moderator collusion.** A single moderator with full paper access can sell it. Historically, insider moderators have been quiet leak sources.
- **L5 — Facility breach.** Lockdown facility isn't a Faraday cage — phones smuggled in, questions memorised (biology especially).
- **L6 — Draft file on shared network.** Even a lockdown room has printers/network storage. Sysadmin becomes a leak vector.

---

## STAGE 3 — Translation 📄

**Who:** Vendor translators (often external agencies) translating the master into 13 languages: Hindi, English, Assamese, Bengali, Gujarati, Kannada, Malayalam, Marathi, Odia, Punjabi, Tamil, Telugu, Urdu.

**How:**
- Each translator receives the (near-final) English master.
- Produces language-specific version, sent back for QA.

```
                       Master (English)
                             │
        ┌───────┬────────┬───┴────┬────────┬────────┐
        ▼       ▼        ▼        ▼        ▼        ▼
      [T-Hi] [T-Ta]   [T-Bn]   [T-Ml]   ...      [T-Ur]
   (each an external vendor / individual translator)
```

### ⚠️ Leak points at Stage 3

- **L7 — Translator leak.** Each translator holds the full paper for days. The number of trusted humans in the loop **multiplies by 13**. This is one of the biggest hidden vulnerabilities.
- **L8 — Translation vendor's storage.** Vendor keeps copies on shared drives.
- **L9 — Roundtrip email/file transfer** without encryption.

---

## STAGE 4 — Master File Creation 📄

**Who:** NTA's technical / production team.

**How:**
- 13 language versions consolidated.
- Multiple **test booklet codes** (E1, E2, E3, E4 and per-language codes) generated — same paper, shuffled question order and option order.
- Final print-ready PDFs (one per language × 4 codes = ~52 masters).

```
        [Master EN]  [Master HI]  ...  [Master UR]
             │            │                 │
             └────────────┴────────┬────────┘
                                   ▼
              Test Booklet Code generator
              (shuffle question order + options)
                                   │
                                   ▼
              52 print-ready PDFs (language × code)
```

### ⚠️ Leak points at Stage 4

- **L10 — Production PC compromise.** The PC assembling the final PDF is a single-point holder of every version of the paper.
- **L11 — Print-ready PDF exfiltration** via USB / cloud sync / email.

---

## STAGE 5 — Printing 📄🖨️

**Who:** Private printing press(es) — outsourced. In NEET-UG 2026, a **Nashik-based press** was implicated.

**How:**
- NTA sends encrypted (in theory) master PDFs to selected press.
- Press decrypts, plates the papers, and runs high-volume printing.
- Prints for ~24M+ paper copies (2.3M candidates × ~10 copies including spares).
- Print run takes several days.

```
     Master PDFs
          │  (theoretically encrypted transfer)
          ▼
   ┌────────────────────────────────────┐
   │   Private Printing Press           │
   │   (e.g. Nashik in NEET-UG 2026)    │
   │                                    │
   │   Operators • Machine minders •    │
   │   Supervisors • QA staff •         │
   │   Housekeeping • Security guards   │
   └────────────────────────────────────┘
          │
          ▼
     Millions of printed booklets
```

### ⚠️ Leak points at Stage 5 (THE BIG ONE — historically)

- **L12 — Press insider scans a copy.** CBI found Nashik press insiders **scanned question papers during the short window between printing and dispatch** in NEET-UG 2026. This is arguably *the* leak in 2026.
- **L13 — Extra print run.** Press prints "spare" copies that don't get destroyed.
- **L14 — Plate / proof retention.** Physical plates or proof copies retained after job done.
- **L15 — Waste-paper leak.** Test prints, misprints, discards not properly incinerated.
- **L16 — Multi-vendor split.** When multiple presses share the job, each becomes a leak vector.

---

## STAGE 6 — Sorting, Packing, Sealing 📄📦

**Who:** Press packing team + NTA supervisors.

**How:**
- Booklets sorted by centre, then by room, then by seat.
- Bundled into sealed packets — one packet per exam room typically.
- Sealed with tamper-evident stickers, packed into steel trunks, addressed to district collectors / nodal banks.

```
   Booklets from press
        │
        ▼
   ┌───────────────────────────────────┐
   │  Packing:                         │
   │   → per candidate booklet         │
   │   → per room packet (~24 books)   │
   │   → per centre carton             │
   │   → per district steel trunk      │
   └───────────────────────────────────┘
        │
        ▼
   Sealed trunks with tamper stickers,
   addressed to district-level custodians
```

### ⚠️ Leak points at Stage 6

- **L17 — Packing floor scan.** Same insider risk as printing — bulk booklets sitting exposed for hours during packing.
- **L18 — Seal replication.** Tamper-evident seals are physical; sophisticated attackers can duplicate a seal, open, scan, reseal.
- **L19 — Miscount / diversion.** "Spare" packets diverted for sale.

---

## STAGE 7 — Transport (Press → Nodal Hub / Bank) 🚚

**Who:** Contracted courier (in NEET-UG 2024: **Blue Dart**) + optional police escort.

**How:**
- Trunks loaded onto trucks / air-freight from press.
- Move from press city to district nodal hubs across the country.
- Multi-day journey. Multiple handovers.

Post-2024 hardening (proposed but not universal):
- Full police escort.
- Indian Air Force transportation for critical hops.
- India Post from nodal hub onwards.
- Real-time GPS tracking.

```
    [PRESS]
      │
      ▼
   ┌──────────────────────────────────────────────┐
   │  Truck / Air-freight (Blue Dart, etc.)       │
   │  Multiple handovers:                         │
   │   Driver → Transit warehouse → Local courier │
   │   → District nodal contact                   │
   └──────────────────────────────────────────────┘
      │
      ▼
   District nodal hub / SBI branch strongroom
```

### ⚠️ Leak points at Stage 7 (THE ONE FROM NEET-UG 2024)

- **L20 — Courier insider access.** Courier staff had unauthorised access to sealed packets. **A 30-minute lapse was enough to digitise a paper.**
- **L21 — Diversion (the Hazaribagh case).** In NEET-UG 2024, courier delivered 9 packets to a **sub-centre in Hazaribagh's Oreya area instead of the designated banks** (CCTV captured e-rickshaw dispatch at 1:30 pm, May 3). Packets sat in an unsecured location.
- **L22 — Vehicle break-in / staged robbery.**
- **L23 — GPS/tracking gap.** Not universally enforced.
- **L24 — Handover-without-witness.** Multi-hop handovers without independent observer.

---

## STAGE 8 — Bank Strongroom / District Storage 🔒📄

**Who:** SBI (or other PSU bank) branch manager + district collector's designate + NTA rep.

**How:**
- Trunks stored in bank strongroom for **days to weeks** before exam.
- Custody notionally shared between bank + district admin + NTA observer.
- Physical logbook of who accessed what, when.

```
   ┌────────────────────────────────────┐
   │       BANK STRONGROOM              │
   │                                    │
   │   Trunks (sealed)                  │
   │   Logbook (paper)                  │
   │                                    │
   │   Access: bank mgr + coll. rep +   │
   │           NTA observer (in theory) │
   └────────────────────────────────────┘
```

### ⚠️ Leak points at Stage 8

- **L25 — Strongroom insider.** Bank employees with keys.
- **L26 — Solo access.** SOP requires 3 keys / 3 signatures; in practice often 1 person opens.
- **L27 — CCTV blind spots or tampered footage.**
- **L28 — Duplicate keys.**
- **L29 — Overnight leak windows** — days of storage give attackers a wide window.

---

## STAGE 9 — Transport (Bank → Exam Centre) 🚚📄

**Who:** District police + centre superintendent's designate.

**How (exam morning):**
- Trunk brought to centre from bank a few hours before exam.
- Signed handover to centre superintendent.
- Packet stored in centre's own strongroom until seal is broken.

```
   Bank Strongroom
       │
       │ (morning of exam)
       ▼
   ┌────────────────────────────────────┐
   │   Police escort (in theory)        │
   │   → Vehicle to centre              │
   │   → Handover with signatures       │
   └────────────────────────────────────┘
       │
       ▼
   Centre Superintendent's custody
```

### ⚠️ Leak points at Stage 9

- **L30 — Vehicle stop.** Escort compromised at a chokepoint.
- **L31 — Wrong centre / redirect.** Physical addressing errors exploited (mirror of L21).
- **L32 — Handover without seal check.** Superintendent signs receipt without verifying seals visually.

---

## STAGE 10 — At the Centre, Pre-Exam 🔒📄

**Who:** Centre Superintendent, Deputy Superintendent, Invigilators (~1 per 24 candidates), Observer.

**How:**
- Trunks in centre strongroom.
- Room-wise packets distributed to invigilators ~15–30 min before start.
- Each packet sealed and marked with room / booklet code.

```
   Centre Strongroom
        │
        ▼
   ┌───────────────────────────────────────┐
   │  Invigilator receives room packet     │
   │  Walks to exam hall                   │
   │  Places sealed packet on front desk   │
   └───────────────────────────────────────┘
```

### ⚠️ Leak points at Stage 10

- **L33 — Invigilator collusion.** Invigilator opens packet in transit or in an empty room, photographs.
- **L34 — Solver-gang plant** — actual invigilator replaced by a collusive one (documented in some state exams).
- **L35 — Delay tactic** — invigilator "misplaces" packet briefly to allow copying.

---

## STAGE 11 — Exam Hall: Seal Break 📄🕐

**Who:** Invigilator, in view of candidates.

**How:**
- **T-5 minutes:** invigilator breaks seal in view of all candidates.
- Booklets distributed.
- Each booklet bears facsimile stamp of Centre Superintendent.
- Candidate verifies booklet code matches OMR code.
- **T-0:** exam starts.

```
    Invigilator (T-5 min)
       │
       │ breaks seal in view of hall
       ▼
   ┌───────────────────────────────────┐
   │   Booklets distributed            │
   │   Candidates verify code match    │
   │   T-0: exam begins                │
   └───────────────────────────────────┘
```

### ⚠️ Leak points at Stage 11

- **L36 — Seal-break-then-leak.** By this point papers are open; if the centre has a mole with a phone in the corridor, T-0 to T-2min photo leak is possible (goes to a WhatsApp group of solvers who dictate answers to candidates via micro-earpiece).
- **L37 — Micro-earpiece / hidden device on candidate.** Answers dictated in real time from an off-site solver who received a photo of the paper.

---

## STAGE 12 — During Exam 📄

**Who:** Candidates, Invigilators, Observer.

**How:**
- ~3h 20min pen-and-paper OMR.
- Invigilator verifies ID (Aadhaar), signs OMR side-1.
- Roving observer.

### ⚠️ Leak points at Stage 12

- **L38 — Impersonation.** Solver sitting for candidate — face doesn't match. (The MP 2023 case: Aadhaar photo swap made this "match.")
- **L39 — Micro-earpiece feed** (from L36).
- **L40 — Bluetooth pen / device.** Documented in multiple state exams.

---

## STAGE 13 — Collection & Sealing of OMR 📄📦

**Who:** Invigilators + Centre Superintendent.

**How:**
- OMRs collected in room, packed, sealed in the presence of candidates (in theory).
- Booklet copies (question papers) collected too, but often distributed later — this is when the paper legally goes public.
- Sealed OMR packets consolidated at centre level.

### ⚠️ Leak points at Stage 13

- **L41 — OMR tampering.** Marks bubbled in after collection by insider before sealing.
- **L42 — OMR photograph** for later dispute manufacture.

---

## STAGE 14 — OMR Transport Back & Scanning 🚚

**Who:** Courier / centre → NTA scanning vendor.

**How:**
- Sealed OMR packets transported back (reverse of Stages 9→7).
- Scanned at central OMR processing facility.
- Digitised responses processed against answer key.

### ⚠️ Leak points at Stage 14

- **L43 — In-transit tampering** of OMR sheets.
- **L44 — Scan vendor insider** modifies digitised responses.
- **L45 — Answer key manipulation** at NTA.

---

## STAGE 15 — Result Publication

**Who:** NTA.

**How:**
- Answer key preliminary release → objection window → final key → results.
- Ranks, cut-offs, admit lists.

### ⚠️ Leak points at Stage 15

- **L46 — Silent rank alteration.** Post-scoring database mutation without audit trail.
- **L47 — Result-before-publish leak** (used to sell "confirmed rank" to universities).

---

## Aggregate leak-vector map

```
STAGE       #    Vectors                             SEVERITY
──────      ──── ───────────────────────────────    ─────────
1 Auth      L1-3   SME device, direct leak, transit  HIGH
2 Moderate  L4-6   Moderator collusion, facility     HIGH
3 Translate L7-9   13× translator vendors            CRITICAL
4 Master    L10-11 Production PC                     MEDIUM
5 Print     L12-16 Press insider (2026 leak)         CRITICAL
6 Pack      L17-19 Packing floor, seal replication   HIGH
7 Transport L20-24 Courier (2024 leak)               CRITICAL
8 Storage   L25-29 Bank strongroom, days of window   HIGH
9 Move      L30-32 Escort compromise                 MEDIUM
10 Centre   L33-35 Invigilator, solver plant         HIGH
11 Break    L36-37 Post-open photo, earpiece         HIGH
12 Exam     L38-40 Impersonation, device             HIGH
13 OMR      L41-42 Bubbling, photo                   MEDIUM
14 Return   L43-45 Scan vendor                       MEDIUM
15 Result   L46-47 Silent alteration                 MEDIUM
```

**Human touch-points on the paper (rough count):**

| Category | Approx count |
|---|---|
| SMEs authoring | ~15–30 |
| Moderation panel | ~6–10 |
| Translators | ~13 |
| Production/QA at NTA | ~5–10 |
| Printing press staff | 50–100+ per press |
| Packing floor | 20–50 |
| Couriers + drivers | 20–50 across country |
| Bank strongroom staff | 3–5 per district × 700+ districts = ~3000 |
| Centre superintendents | ~5000 |
| Invigilators | ~100,000 |
| Observers | ~10,000 |

**~ 120,000+ humans touch the paper before a candidate sees it.** Any one can leak. The current system assumes each is trustworthy. It shouldn't.

---

## Where the redesign (SetuExam) closes each vector

| Vector class | Current failure mode | SetuExam replacement |
|---|---|---|
| L1–L11 (authoring → master) | Plaintext exists on humans' devices | Air-gapped authoring, threshold-encrypted commit at each stage |
| L12–L19 (printing/packing) | Plaintext printed, exposed to press insiders | **Skip printing entirely — CBT** — no plaintext ever exists outside T-0 session |
| L20–L24 (transport) | Physical courier vulnerable | Ciphertext-only transport; useless without threshold key |
| L25–L29 (strongroom) | Insider access over days | Ciphertext-only, decryption impossible until T-0 threshold ceremony |
| L30–L32 (centre delivery) | Physical intercept | Not applicable in CBT — nothing physical to intercept |
| L33–L37 (opening/seal-break) | Human opens seal, mole photographs | Per-candidate shuffle via VRF — no single "the paper" to photograph |
| L38 (impersonation) | Single-DB Aadhaar can be tampered | Multi-issuer W3C VCs — must corrupt ≥3 issuers simultaneously |
| L39–L40 (in-exam devices) | Micro-earpiece, Bluetooth | Camera-based AI proctoring + jammer at centre (unchanged — cryptography doesn't solve this) |
| L41–L42 (OMR tamper) | Physical OMR mutable | Digital answers encrypted + hash-committed on-chain per submission |
| L43–L45 (scan vendor) | Vendor insider mutates | Grading under attested enclave; results signed and on-chain |
| L46–L47 (result mutation) | Silent DB edit | Immutable on-chain result commitment; VC issued to candidate wallet |

## The single biggest insight

The current lifecycle has **~120,000 human trust points across 15 stages over ~90 days**. Each is a leak vector. The 2024 leak was at stage 7. The 2026 leak was at stage 5. The 2027 leak will be at some other stage.

**The only sustainable fix is to remove the plaintext paper from every stage until T-0.** That's what threshold encryption + CBT delivers by construction — not by trust.

---

## Sources

- https://www.edufever.com/neet-ug-2025-exam-how-question-papers-are-set-all-you-need-to-know/
- https://kaullege.com/neet-ug-how-question-papers-are-set-all-you-need-to-know/amp/
- https://ask.shiksha.com/who-sets-the-neet-question-paper-qna-7436713
- https://www.tribuneindia.com/news/india/paper-setters-in-lockdown-5l-security-staff-nta-big-steps-to-make-neet-retest-leak-proof/
- https://www.printweek.in/article/neet-ug-2026-leak-exposes-printing-press-weak-links/4saespfvmeksgrg2yr3jn94fdy
- https://organiser.org/2026/06/07/356980/bharat/neet-ug-re-exam-security-overhaul-government-mulls-budget-press-printing-iaf-transportation-to-prevent-paper-leaks/
- https://m.dailyhunt.in/news/india/english/india+employment+news-epaper-indemnew/neet+ug+2026+where+are+exam+papers+for+tests+like+neet+printed+and+how+do+they+get+leaked+from+there-newsid-n712201036
- https://www.eklavvya.com/blog/secure-question-paper-generation-after-neet-2026-leak/
- https://www.tribuneindia.com/news/india/neet-ug-probe-printing-press-to-exam-centres-paper-custody-chain-under-cbi-radar-633758
- https://www.deccanherald.com/india/neet-ug-2025-transport-of-question-papers-under-police-escort-monitoring-of-coaching-centres-3514044
- https://www.careerindia.com/news/neet-ug-retest-in-jhajjar-haryana-amid-allegations-of-paper-leak-041753.html
- https://www.vedantu.com/neet/neet-rules-and-regulations
- https://motion.ac.in/examinfo/neet-exam-day-guidelines/
- https://www.prepexams.in/NEET/neet-instruction-exam-hall.html
- https://www.stvea.in/NEET-Exam-Day-Guidelines

# Blockchain for Exam Paper Integrity — What Works, What's Cargo-Culted

_Fair warning: most "blockchain exam" papers are undergraduate-tier. Below distills the parts that are actually useful for a real deployment._

## What blockchain genuinely provides here

1. **Immutable audit trail.** Every custody event (encrypt → hand-off → transport → open → decrypt → destroy) becomes a signed, timestamped, unchangeable log entry. This is the single strongest fit.
2. **Public verifiability of process, not content.** Anyone (media, courts, students) can verify "the paper hash was committed at T-14 days, decryption keys were released at T-0, no earlier access is possible" — without seeing the paper.
3. **Removal of single custodian.** Multi-signature / threshold schemes distribute trust across independent parties (NTA + independent auditor + judiciary observer + state rep).
4. **Time-locked / conditional access.** Smart contracts release decryption keys only after conditions (block height, oracle-attested time, biometric checks passed) are met — no "trusted admin" who could release early.
5. **Traceable access attempts.** Every attempted decryption event is logged. Unauthorised attempts become detectable.

## What blockchain does NOT solve (be honest about this)

- **The paper on paper.** If it's a physical PDF that leaks after decryption, blockchain doesn't help. The leak vector needs to be the CBT screen, not a printed page.
- **Insider printing.** If the question authors themselves leak, no ledger helps. Address this via **secret splitting + n-of-m authoring** (see 04-cryptography).
- **Camera phones at centres.** Physical shoulder-surfing / hidden phones is a proctoring problem, not a blockchain problem.
- **Storage.** You would NOT store papers on-chain. Ciphertext lives on encrypted IPFS / private storage; only hashes and access events go on-chain.

## Reference architectures from literature

### A. "SafePaper" — blockchain + Shamir's Secret Sharing
- Question paper encrypted; encryption key split via **Shamir's Secret Sharing** into n shares.
- Shares distributed to independent authorities (NTA officer, external auditor, judicial rep, etc.).
- Any t-of-n must come online near T-0 to reconstruct — no single party can leak.
- Blockchain records each share issuance and reconstruction event.

### B. Smart-contract time-locked distribution
- Ethereum/Hyperledger contract holds hash + encrypted key.
- Key becomes releasable only after block/time condition.
- Every access is a transaction — auditable.

### C. IPFS + blockchain access control (proven pattern from health records)
- Encrypted paper → IPFS (content-addressed, immutable, distributed).
- CID hash → blockchain.
- Smart contract enforces "which key can decrypt which CID and when."
- AES-256 for content, ECDH for key exchange, SHA-256 for integrity.

## Choice of ledger

| Option | Fit for this use case |
|---|---|
| **Public L1 (Ethereum, Polygon)** | Best for public verifiability — students/media/court can verify without permission. Cost per event is trivial (custody events are low-frequency). |
| **Hyperledger Fabric** | Common in Indian govt PoCs (already used by NIC in some pilots). Permissioned, higher throughput, but weaker public-verifiability story. |
| **Polygon zkEVM / Optimism** | Sweet spot — public L1 verifiability, low cost, EVM tooling. |
| **Cosmos / app-chain** | Overkill unless building a full exam-chain. |

**Recommendation for prototype:** Polygon (EVM, cheap, public), with the option to move to a **consortium chain** for center-level events at scale. Public verifiability is a political requirement post-CJP — if only NTA can read the ledger, the trust story dies.

## What to put on-chain vs off-chain

| On-chain (small, high-value) | Off-chain (encrypted, distributed) |
|---|---|
| Hash of every version of the paper | The paper ciphertext itself (IPFS/private storage) |
| Public keys of authorities | Private key shares (in HSMs of each authority) |
| Custody-chain events (signed, timestamped) | CCTV / centre metadata |
| Decryption key release events | Candidate biometric templates |
| DID documents of centres / authorities | Answer sheets (encrypted, blockchain-anchored) |

## Anti-patterns to avoid

- ❌ Storing the paper on-chain (obvious but people propose it).
- ❌ Using blockchain as a *database*.
- ❌ Assuming smart contracts replace SOPs — they *encode* them; SOP redesign still required.
- ❌ Permissioned chain with only NTA validators — kills the trust story.
- ❌ Hand-rolling threshold crypto in a smart contract — use battle-tested libs (see 04-cryptography).

## Sources

- https://medium.com/@wordgasm367/resolving-exam-paper-leaks-with-blockchain-5b07091416f2
- https://www.ijirmps.org/papers/2026/1/232926.pdf (PAPER LEAKAGE PREVENTION USING BLOCKCHAIN)
- https://www.academia.edu/143141420/SafePaper_Blockchain_Based_Secure_Examination_Paper_Storage_with_Shamirs_Secret_Sharing
- https://www.jetir.org/view?paper=JETIR2507308 (Question Paper Prevention System Using Blockchain)
- https://www.antiersolutions.com/blogs/implementing-blockchain-solutions-can-prevent-leaks-of-exam-question-paper/
- https://ijsrem.com/download/blockchain-framework-for-securing-and-distributing-exam-papers/
- https://www.ijirset.com/upload/2024/august/80_Blockchain.pdf
- https://www.ijmrset.com/upload/101_Question%20Paper.pdf

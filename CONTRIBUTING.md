# Contributing to SetuExam

Thank you for wanting to help. Read this once; it's short.

## Ground rules

1. **Assume good faith.** Contributors come from cryptography, blockchain, government, coaching, teaching, law, journalism, and the student body itself. Different vocabularies, same goal.
2. **Cite everything.** Every factual claim in the docs must cite a public source. If it's your own analysis, mark it as such.
3. **No PII, ever.** Do not commit anyone's real name, phone, Aadhaar, application number, or exam roll. Not in examples, not in test data, not in commits.
4. **Security research is welcome, exploitation is not.** If you find a real weakness in a live exam pipeline, report it responsibly to NTA / CERT-In. This repo is for the *design*, not for attacking live systems.

## How to contribute

### Docs / research
1. Fork the repo.
2. Add or edit files in the relevant `NN-*` folder.
3. Each factual claim needs a source URL at the bottom of the doc.
4. Open a PR with a one-line summary and a two-line rationale.

### Code (once we start shipping)
1. Fork, branch off `main`.
2. Follow the language-specific style in each subdir (e.g., `solc --formatter` for Solidity, `gofmt` for Go, `rustfmt` for Rust).
3. Write tests. Cryptographic code without tests will be rejected.
4. Open a PR; a maintainer will review.

### Translations
1. Copy `README.md` to `README.<lang>.md` (e.g., `README.hi.md`).
2. Translate. Keep code blocks, file paths, and technical terms as-is (or add gloss).
3. PR it.

### Issues
- Use the templates.
- Anonymous / pseudonymous issues welcome — especially from anyone inside the current exam pipeline. We will never require you to reveal your identity.

## Commit messages

Short imperative. Reference the doc/component. Examples:
- `docs(cryptography): add TR-CSS section from IACR 2016`
- `contracts: scaffold ThresholdCeremony.sol`
- `translate: add README.ta.md`

## PR review

- All PRs need one approving review before merge.
- Cryptographic PRs need a review from someone tagged `crypto-reviewer`.
- Doc-only PRs can be self-merged after 48h if no objections.

## Code of Conduct

Be kind. Assume good faith. Attack ideas, not people. See [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## Governance (draft)

For now, decisions happen in PR discussion and GitHub Issues. As the project grows, we'll add a formal RFC process and a rotating maintainers group with representatives from crypto, blockchain, identity, policy, and student community.

## Licensing

By contributing you agree that:
- Your code contributions are under [MIT](LICENSE-CODE).
- Your doc contributions are under [CC BY 4.0](LICENSE-DOCS).
- You have the right to contribute the material (it's your own work, or already open under a compatible licence).

That's it. Ship.

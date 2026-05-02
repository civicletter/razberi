# Razberi — open-source assistant for understanding Bulgarian state correspondence

> *razberi* (разбери) — "to figure out", in Bulgarian, Russian, and Ukrainian.

**Status:** active research, pre-alpha. Targeting NLnet NGI Zero Commons Fund (June 2026 round).

## What it is

Razberi is an open-source software stack that helps people who do not read Bulgarian
make sense of letters from the Bulgarian state — the State Agency for Refugees (SAR /
ДАБ), the National Revenue Agency (NRA / НАП), and the Migration Directorate of the
Ministry of Interior (Дирекция "Миграция").

A user uploads a photo or PDF of a letter to a Telegram bot or web form. Razberi:
1. Performs OCR on Cyrillic forms and stamps (PaddleOCR PP-OCRv5).
2. Classifies the document type against a curated taxonomy.
3. Retrieves the relevant Bulgarian legal articles via RAG over a versioned corpus.
4. Returns a plain-language explanation in Russian or Ukrainian, with the deadline,
   the action required, and a draft reply.

## Why it matters

Bulgaria currently hosts ~75,000 Ukrainians under Temporary Protection (UNHCR, 2024),
plus thousands of Russian and Belarusian relocants. Most of them do not read Bulgarian.
A single missed letter from SAR or NRA can mean the loss of legal status or a fine.
Existing tools — Google Translate, ChatGPT in a chat group, paid migration lawyers —
either hallucinate on regulatory text or are inaccessible. Razberi provides the
missing public infrastructure: grounded, auditable, locally-deployable, free.

## What we publish (deliverables, not promises)

- `bg-state-letters` — anonymised dataset of Bulgarian government letters (CC-BY-SA 4.0).
- `bg-pii-redact` — Bulgarian/Cyrillic identity redaction library, on top of Microsoft Presidio.
- `bg-state-rag` — RAG pipeline indexing ZVPOO, ZChRB, ZDDFL, and Council of Ministers decisions.
- `razberi-api` — FastAPI service with pluggable LLM router (Mistral, OpenRouter, self-hosted Llama-3.1).
- `razberi-bot` — Telegram-bot reference client (aiogram 3).

## Architecture

[ASCII diagram of the pipeline — copy from §3.1 of the project plan]

## Stack

Python 3.12 · FastAPI · PaddleOCR · LlamaIndex · pgvector on Postgres 16 ·
Microsoft Presidio · aiogram 3 · Mistral La Plateforme (default) · OpenRouter ·
Llama-3.1-8B-Instruct via vLLM (self-host fallback) · Hetzner Cloud (EU).

## Open source

- Code: AGPL-3.0
- Dataset: CC-BY-SA 4.0
- Documentation, prompts, templates: CC-BY 4.0
- SDK / client libraries: Apache-2.0

We follow [REUSE.software](https://reuse.software/) for SPDX headers.

## Governance

Razberi is currently led by the Project Founder (BDFL phase) with a planned transition to a multi-stakeholder Steering Committee. See [GOVERNANCE.md](GOVERNANCE.md) for details.

## GenAI policy

This project complies with [NLnet Foundation's GenAI Policy v1.1](https://nlnet.nl/foundation/policies/generativeAI/).
GenAI was used to assist drafting of documentation and the grant application; prompt logs are
maintained in `docs/genai-policy.md`. All code is human-reviewed and human-tested.

## Roadmap

- [ ] M1: Corpus + redaction toolkit (≥150 letters)
- [ ] M2: OCR + document classifier
- [ ] M3: RAG pipeline grounded in Bulgarian legislation
- [ ] M4: Pluggable LLM router + alpha API
- [ ] M5: Self-hosted Llama-3.1 reference deployment
- [ ] M6: Telegram-bot reference client + WCAG-AA web client
- [ ] M7: Public beta + technical report + sustainability handoff

## How to help

- Submit a (consent-given, anonymised) state letter to expand the corpus: see [CONTRIBUTING.md](CONTRIBUTING.md).
- Test the alpha bot when it launches.
- Translate UI strings to your language.

## Contact

- Mastodon: [@razberi@fosstodon.org](https://fosstodon.org/@razberi) (TBD)
- Email: [TBD]@razberi.bg
- GitHub Issues for technical conversation.

## License

[GNU Affero General Public License v3.0](LICENSE)

---
*Razberi — Grounded, Auditable, Free.*

See also: [GOVERNANCE.md](GOVERNANCE.md) | [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)

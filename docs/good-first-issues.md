# Good First Issues

Below are the drafted texts for 5 "good first issues". You can copy and paste these into GitHub when you are ready to open them.

---

## 1. [corpus] Define document-type taxonomy schema (JSON Schema)

**Labels:** `good first issue`, `documentation`, `corpus`

**Description:**
As part of our dataset curation for `bg-state-letters`, we need a standardized way to label the types of documents we receive (e.g., "SAR Decision", "NRA Fine", "Migration Visa"). Currently, we don't have a strict schema for the metadata `.json` files that accompany each image/PDF. 

We need a JSON Schema definition that enforces the structure of our document taxonomy.

**Acceptance Criteria:**
- Create a `schema.json` file in `data/taxonomy/`.
- The schema must define a required `document_type` enum (e.g., `sar_decision`, `nra_notice`).
- Include fields for `issuing_agency`, `date_received`, and `language`.
- Provide a brief `README.md` in `data/taxonomy/` showing an example of a valid JSON metadata file.

---

## 2. [ocr] Benchmark PaddleOCR PP-OCRv5 on Cyrillic with overlapping stamps

**Labels:** `good first issue`, `research`, `ocr`

**Description:**
Razberi relies on PaddleOCR (PP-OCRv5) to extract text from photos of state letters. Bulgarian state letters frequently contain overlapping blue ink stamps over Cyrillic text, which can degrade OCR performance.

We need a baseline benchmark to understand how well PaddleOCR handles these specific artifacts.

**Acceptance Criteria:**
- Create a simple Python script `scripts/benchmark_ocr.py`.
- The script should run PaddleOCR PP-OCRv5 on a set of 5-10 sample images (you can use placeholders or publicly available stamped Cyrillic documents).
- Output a markdown report or CSV containing the expected text vs. extracted text, and a basic accuracy score (e.g., Character Error Rate or Levenshtein distance).
- Document instructions on how to run the benchmark locally.

---

## 3. [redact] Initial PII recognizers for ЕГН, ЛНЧ, BG passport, BG IBAN

**Labels:** `good first issue`, `privacy`, `redact`

**Description:**
To safely build our open-source dataset, we use Microsoft Presidio to redact Personally Identifiable Information (PII). While Presidio handles general entities well, it lacks specific recognizers for Bulgarian identification numbers.

We need to implement custom Regex-based recognizers for Bulgarian formats.

**Acceptance Criteria:**
- Create a Python module `src/razberi/redact/bg_recognizers.py`.
- Implement `PatternRecognizer` classes for:
  - ЕГН (EGN - 10 digits).
  - ЛНЧ (LNCh - 10 digits).
  - BG Passport/ID Card numbers (typically 9 digits).
  - BG IBAN (Starts with `BG` + 20 alphanumeric characters).
- Add basic unit tests using `pytest` in `tests/test_redaction.py` to ensure valid numbers are flagged and redacted.

---

## 4. [rag] Ingest ZVPOO + Council of Ministers Decision No. 79/2025 into pgvector

**Labels:** `good first issue`, `backend`, `rag`

**Description:**
Our RAG pipeline requires a grounded vector database of Bulgarian legal texts. We are starting with the Law on Asylum and Refugees (ЗУБ/ЗВПОО) and the recent Council of Ministers Decision No. 79/2025 (Temporary Protection extension).

We need a script to chunk these texts and ingest them into our Postgres `pgvector` database using LlamaIndex.

**Acceptance Criteria:**
- Create an ingestion script `scripts/ingest_laws.py`.
- Use LlamaIndex to parse markdown or txt versions of the specified laws.
- Implement an ingestion pipeline that chunks the documents (e.g., by article/paragraph) and generates embeddings.
- Save the embeddings to a local `pgvector` instance.
- Provide a `docker-compose.yml` snippet (or instructions) for spinning up a local Postgres + pgvector instance for testing.

---

## 5. [docs] Add architecture diagram (Mermaid)

**Labels:** `good first issue`, `documentation`

**Description:**
Our `docs/architecture.md` currently has an ASCII diagram representing the MVP architecture. To make it more maintainable and visually appealing, we want to convert this ASCII diagram into a Mermaid.js flowchart.

**Acceptance Criteria:**
- Update `docs/architecture.md`.
- Replace the ASCII diagram with a ````mermaid ... ```` block.
- The Mermaid diagram must accurately reflect the flow:
  - Telegram/Web UI -> FastAPI
  - FastAPI -> PaddleOCR, Presidio, Classifier, RAG (Postgres/pgvector), LLM Router
  - LLM Router -> Mistral, OpenRouter, Local Hetzner GPU.
- Ensure the diagram renders correctly on GitHub.

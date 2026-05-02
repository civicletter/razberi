3.1. MVP Architecture
[Telegram bot (aiogram 3) / Web UI (Next.js)]
            │
            ▼
[FastAPI backend, Python 3.12]
   ├─ OCR: PaddleOCR PP-OCRv5 (Apache-2.0, supports Cyrillic + 109 langs)
   │       fallback: docTR (for specific layouts)
   ├─ PII redaction: presidio-analyzer + custom regex for EGN/LNCH/passport
   ├─ Document classifier: small-model classification (sentence-transformers
   │       multilingual-e5-small, fine-tuned on the collected corpus)
   ├─ RAG: LlamaIndex (MIT) over pgvector in Postgres 16
   │       indexing: laws (ZVPOO, ZChRB, ZDDFL), Council of Ministers regulations,
   │       and custom explained templates
   ├─ LLM router (open-source: pluggable):
   │       Tier 1 (default): Mistral Small 3 / Mistral Large via API
   │       Tier 2: Llama-3.1-70B-Instruct via OpenRouter
   │       Tier 3 (self-host fallback): Llama-3.1-8B locally on Hetzner GPU
   └─ Audit log: every explanation is stored with the model version signature
            │
            ▼
[Hetzner Cloud — Falkenstein/Helsinki, GDPR-friendly]
   docker-compose: api, postgres+pgvector, paddle-ocr, llm-proxy, traefik

The system is designed as a modular pipeline that processes document uploads through several specialized stages. Initially, **PaddleOCR** extracts text while preserving multilingual context, followed by **Presidio** for PII redaction to ensure privacy compliance. The refined data is then categorized by a fine-tuned transformer model and passed to the **RAG (Retrieval-Augmented Generation)** engine. This engine queries a high-performance **pgvector** database containing localized legal statutes and internal templates, providing the LLM with the necessary legal context for accurate response generation.

Decision-making is orchestrated by a pluggable **LLM Router**, which dynamically selects the most appropriate model based on task complexity and availability. By utilizing a tiered approach—ranging from high-performance APIs like **Mistral Large** to self-hosted **Llama 3.1** instances on **Hetzner GPU** nodes—the architecture maintains a balance between cost-efficiency and data sovereignty. The entire stack is containerized via **Docker Compose** and deployed in GDPR-friendly regions, ensuring that all audit logs and model signatures are handled within secure, compliant infrastructure.

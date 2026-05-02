# Contributing to Razberi

Thank you for your interest in contributing to Razberi! We are building open-source infrastructure to help people navigate Bulgarian state correspondence. Whether you are a developer, a translator, or someone who has received a letter from the state, your help is invaluable.

## 1. Submitting Letter Samples

The heart of Razberi is our corpus of Bulgarian state letters. To improve our OCR and classification models, we need a diverse set of samples from various agencies (SAR, NRA, Migration, etc.).

### Consent and Privacy (Mandatory)
Before submitting a letter, you **must** ensure the following:
* **Consent**: You must have the explicit permission of the letter recipient to share the document.
* **Anonymization**: While we use `bg-pii-redact` for automated redaction, we strongly encourage you to manually black out names, addresses, EGN/LNCH numbers, and phone numbers before uploading.

### How to Submit
1. **GitHub PR**: Add your anonymized image or PDF to the `data/samples/` directory. Include a small `.json` file with the same name describing the document type (e.g., "Decision on refugee status").
2. **Email**: Send your samples to `samples@razberi.bg` (TBD) if you prefer us to handle the initial anonymization and integration.

## 2. Reporting Issues

We use GitHub Issues to track bugs and feature requests.

* **Bug Reports**: Please include your OS, Python version, and the specific document type you were processing when the error occurred.
* **Taxonomy Gaps**: If you find a Bulgarian state document that Razberi fails to classify, open an issue with the subject "Taxonomy: [Agency Name] - [Document Title]".
* **RAG Hallucinations**: If the bot provides a legal explanation that contradicts the provided Bulgarian text, please flag it immediately.

## 3. Development and Testing

Razberi is built with Python 3.12 and FastAPI.

### Local Setup
```bash
git clone https://github.com/civicletter/razberi.git
cd razberi
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt  # TBD
```

### Running Tests
We use `pytest` for our test suite. Before submitting a Pull Request, ensure all tests pass:

```bash
# Run all tests
pytest

# Run tests for a specific module (e.g., redaction)
pytest tests/test_redaction.py
```

If you are adding a new feature, please include corresponding test cases in the `tests/` directory.

### Manual Verification
For changes related to OCR or RAG, please perform manual verification using the scripts in `scripts/`:
```bash
python scripts/verify_ocr.py --input data/samples/test_letter.jpg
```

## 4. Coding Standards

* **REUSE Compliance**: All files must have SPDX license headers.
* **Type Hints**: Use Python type hints for all function signatures.
* **GenAI Usage**: If you use GenAI to assist in writing code, you must review it and log the usage in `docs/genai-policy.md` if the change is significant.

---
*Razberi — Grounded, Auditable, Free.*

See also: [GOVERNANCE.md](GOVERNANCE.md) | [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)

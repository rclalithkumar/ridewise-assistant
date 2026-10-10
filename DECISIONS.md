# RideWise AI Assistant — Engineering Decisions

## 1. Three Important Design Decisions

### Decision 1: Use SQLite for structured rental data

**Decision:** Retrieve relevant booking and payment records using SQL queries with parameters.

**Why:** Structured records need accurate filtering. Parameterized queries help prevent SQL injection, and customer-scoped retrieval helps protect customer data.

**Alternative rejected:** Asking the LLM to answer directly from the entire database, which would weaken record-level control and reliability.

### Decision 2: Combine database retrieval with policy retrieval

**Decision:** Use SQLite for operational records and keyword-overlap retrieval from approved policy PDFs, supplemented by verified text for scanned annexures.

**Why:** Booking and payment questions require exact records, while policy questions require relevant clauses and traceable sources.

**Alternative rejected:** Searching every PDF indiscriminately or relying only on the LLM's pretrained knowledge.

### Decision 3: Use the supplied NeoStats-hosted LLM

**Decision:** Use the authorized OpenAI-compatible endpoint and load credentials from local environment variables.

**Why:** This follows the hackathon requirements without introducing an unrelated model provider.

**Alternative rejected:** Switching to another hosted model or adding unnecessary infrastructure.

## 2. Development Failures and Resolutions

### Policy retrieval regular-expression error

**Problem:** An incorrectly escaped regular-expression pattern caused an unterminated character-set error.

**Resolution:** Corrected the pattern and checked Python syntax.

### Unrelated PDF retrieval risk

**Problem:** Searching every PDF could mistakenly treat the hackathon use-case document as an official policy source.

**Resolution:** Restricted retrieval to the five approved policy PDFs and handled verified scanned-annexure text separately.

### Unsupported answers

**Problem:** The LLM could make claims beyond the retrieved evidence.

**Resolution:** Added evidence-grounding instructions and manually tested unsupported battery-health claims, refund guarantees, and requests for private contact information. These manual tests passed during development.

### Role selection and customer privacy

**Problem:** The session flow needed to support both Customer and Support Agent evaluation without password-based login.

**Resolution:** Added role selection, customer ID validation, and a new-conversation flow. Customer privacy, Support Agent access, and new-conversation behavior passed manual functional checks. This demonstration is not production-grade authentication.

## 3. What I Would Improve With One More Week

1. **Automated testing:** Add repeatable tests for customer isolation, booking retrieval, policy version selection, and citation correctness.
2. **Authorization hardening:** Enforce and test role permissions independently of LLM prompt instructions.
3. **Retrieval evaluation:** Create a test set to measure evidence relevance, citation accuracy, and unsupported-answer rates.
4. **Policy version handling:** Test the July 1, 2026 cutoff and verify that the applicable policy is selected using the correct booking date.
5. **Observability:** Improve error handling and latency measurement while avoiding API keys and unnecessary customer data in logs.
6. **Production readiness:** Review authentication, secret management, audit requirements, and data retention before handling real customer information.

## 4. Known Limitations

- Keyword-overlap retrieval can miss relevant evidence when questions use unexpected terminology.
- Scanned-document transcriptions require verification against original policy pages.
- Prompt instructions reduce hallucination risk but cannot guarantee correct answers.
- Role selection and customer ID validation do not replace production-grade authentication and authorization.
- The prototype requires further security, reliability, and retrieval testing before production deployment.

The priorities are correctness, privacy, evidence traceability, and explainability.

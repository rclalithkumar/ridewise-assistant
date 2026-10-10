\# RideWise AI Assistant — Engineering Decisions



\## 1. Three Important Design Decisions



\### Decision 1: Use SQLite for structured rental data



\*\*Decision:\*\* Retrieve customer, booking, payment, vehicle, and damage records using SQL queries with parameters.



\*\*Why:\*\* These records have a structured schema and require accurate filtering. Parameterized queries help prevent SQL injection, while customer-scoped retrieval reduces unauthorized data exposure.



\*\*Alternative rejected:\*\* Asking the LLM to answer directly from the entire database. This would provide weaker access control and less reliable record-level answers.



\### Decision 2: Combine SQL retrieval with policy document retrieval



\*\*Decision:\*\* Use SQLite for transactional records and retrieval from an explicit allowlist of official policy PDFs, supplemented by verified text for scanned annexures.



\*\*Why:\*\* Database questions require exact records, while policy questions require relevant clauses and citations. Explicit source selection prevents unrelated documents from being treated as official policy evidence.



\*\*Alternative rejected:\*\* Searching every PDF in the folder or relying only on the LLM's pretrained knowledge.



\### Decision 3: Use the supplied NeoStats-hosted LLM



\*\*Decision:\*\* Call the provided OpenAI-compatible endpoint using the hackathon's configured model and environment variables.



\*\*Why:\*\* This follows the challenge requirements and avoids introducing an unrelated external model provider.



\*\*Alternative rejected:\*\* Switching to a different hosted model or adding unnecessary infrastructure.



\## 2. Development Failures and Resolutions



\### Policy retrieval regex error



\*\*Problem:\*\* A regular-expression pattern contained incorrectly escaped backslashes, causing an unterminated character-set error.



\*\*Resolution:\*\* Corrected the raw-string patterns used to split source sections and extract filename/page citations, then checked Python syntax.



\### Unrelated PDF retrieval risk



\*\*Problem:\*\* Searching every PDF in the policy folder could include the hackathon use-case document as if it were an official policy.



\*\*Resolution:\*\* Restricted retrieval to the five official policy PDFs and included a separate verified transcription of the scanned annexures.



\### Unsupported answers



\*\*Problem:\*\* The LLM could potentially make claims beyond the retrieved evidence.



\*\*Resolution:\*\* Added evidence-grounding instructions and tested unsupported battery-health claims, refund guarantees, and requests for private contact information. The three manual tests passed during development.



\## 3. What I Would Improve With One More Week



1\. \*\*Automated testing:\*\* Add repeatable tests for customer isolation, booking retrieval, policy version selection, and citation correctness.

2\. \*\*Authorization hardening:\*\* Enforce support-agent permissions in application logic and test every data-access path independently of the LLM prompt.

3\. \*\*Retrieval evaluation:\*\* Build a curated question-and-answer test set to measure retrieval relevance, citation accuracy, and unsupported-answer rates.

4\. \*\*Policy version handling:\*\* Add explicit tests around the July 1, 2026 cutoff and ensure booking creation time, rather than cancellation time, determines the applicable policy.

5\. \*\*Observability and reliability:\*\* Improve error handling, latency measurement, and safe logging without recording API keys or unnecessary customer information.

6\. \*\*Production readiness:\*\* Review authentication, secret management, audit requirements, and data retention before using real customer information.



\## 4. Known Limitations



\- Retrieval may miss relevant evidence if a question uses unexpected terminology.

\- Scanned-document transcriptions require verification against the original policy pages.

\- Prompt instructions reduce hallucination risk but cannot guarantee that every LLM response is correct.

\- The prototype requires further security, authorization, and reliability testing before production deployment.



The priorities are correctness, privacy, evidence traceability, and explainability.

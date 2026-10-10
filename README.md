\# RideWise AI Assistant



An evidence-grounded customer support assistant for a fictional electric vehicle rental service. It combines structured rental records, official policy documents, and a NeoStats-hosted large language model (LLM) to answer customer questions.



\## Features



\- Streamlit chat interface with Customer and Support Agent roles.

\- SQLite retrieval for customer, booking, payment, vehicle, and damage records.

\- Policy retrieval from the approved rental and policy documents.

\- Answers grounded in retrieved evidence, with policy filename and page citations where available.

\- Customer-scoped database queries and parameterized SQL.

\- Explicit handling of missing evidence and unsupported guarantees.

\- API credentials loaded from local environment configuration.



\## Architecture



1\. \*\*Streamlit UI:\*\* Accepts the user's question and selected role.

2\. \*\*Authorization and SQLite retrieval:\*\* Retrieves relevant structured records while applying customer-scoping controls.

3\. \*\*Policy retrieval:\*\* Searches approved policy PDFs and verified text transcriptions of scanned annexures.

4\. \*\*NeoStats-hosted LLM:\*\* Generates a natural-language response using the retrieved evidence.

5\. \*\*Evidence-based response:\*\* Communicates supported findings, policy citations, or limitations when evidence is insufficient.



\## Requirements



\- Python 3.11 recommended

\- Access to the NeoStats hackathon LLM endpoint

\- The supplied RideWise SQLite database and approved policy documents



\## Local setup



1\. Clone or obtain the project files.

2\. Create and activate a Python virtual environment if needed.

3\. Install the project's Python dependencies.

4\. Copy `.env.example` to `.env`.

5\. Configure the API key locally using the credentials supplied for the hackathon.

6\. Start the application:



&#x20;  `python -m streamlit run app.py`



Do not commit `.env` or share its credentials.



\## Configuration



The `.env.example` file documents these settings:



\- `NEOSTATS\_API\_KEY`: your private API key

\- `NEOSTATS\_BASE\_URL`: NeoStats-compatible API endpoint

\- `NEOSTATS\_MODEL`: configured model or deployment name



Use the exact endpoint and model configuration provided for the hackathon.



\## Suggested test scenarios



\- Retrieve the status of a known booking.

\- Retrieve payments for the selected customer.

\- Ask about cancellation rules for a booking made after July 1, 2026.

\- Test GOLD-tier late-return rules.

\- Ask about the low-battery penalty.

\- Request information not present in the evidence.

\- Attempt to access another customer's records.

\- Ask for an unsupported refund guarantee or private personal contact information.



\## Privacy and evidence grounding



Database queries use parameters and are scoped to the selected customer where applicable. Role selection alone must not be treated as authorization to expose unrestricted customer records. The application instructs the LLM to use retrieved evidence, avoid inventing policy terms or amounts, and acknowledge missing information.



These controls reduce risk but do not guarantee that every generated answer will be correct. Validate access controls and evidence before using the application with real customer data.



\## Limitations



\- Responses depend on the completeness and relevance of retrieved records and policy passages.

\- Scanned-document transcriptions must be checked against the original documents.

\- LLM output may still contain errors and should be validated for consequential decisions.

\- This hackathon prototype has not been established as a production-ready customer support system.



\## Data handling



Include the supplied database and policy documents in a shared submission only if permitted by the hackathon rules. Never commit API keys or other secrets.

# RideWise AI Assistant

An evidence-grounded AI support assistant for a fictional electric vehicle rental service. It combines SQLite records, approved policy documents, and the NeoStats-hosted LLM to answer questions using retrieved evidence.

## Features

- Streamlit chat interface with Customer and Support Agent roles.
- Customer ID validation against the supplied database.
- Customer-scoped booking and payment retrieval.
- Support Agent access to operational information.
- Keyword-overlap retrieval from approved policy PDFs and verified scanned-annexure text.
- Evidence-grounded LLM responses with policy filename and page citations where available.
- Explicit handling of missing evidence and unsupported claims.
- New-conversation controls.
- Local environment configuration for API credentials.

## Architecture

1. **Streamlit UI:** Role selection, chat, and session controls.
2. **Session handling:** Validates customer IDs and initializes the selected role.
3. **SQLite retrieval:** Uses parameterized SQL to retrieve relevant operational records.
4. **Policy retrieval:** Ranks passages using keyword overlap from approved policy sources.
5. **NeoStats LLM:** Generates answers using retrieved evidence.
6. **Evidence display:** Shows database evidence and relevant policy passages.

## Technology stack

- Python 3.11
- Streamlit
- SQLite
- PyMuPDF
- OpenAI-compatible Python client
- python-dotenv
- NeoStats-hosted LLM endpoint

## Requirements

- Python 3.11 recommended
- Authorized access to the NeoStats hackathon LLM endpoint
- The organizer-provided `ridewise.db`
- The approved policy documents in their expected locations

## Local setup

Open PowerShell in the project directory.

Create and activate a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install dependencies:

```powershell
python -m pip install -r requirements.txt
```

Create your local environment file:

```powershell
Copy-Item .env.example .env
```

Edit `.env` and enter the authorized API key and endpoint settings supplied for the hackathon. Never commit `.env` or share credentials.

Ensure that `ridewise.db` and the approved policy files are available in their expected locations.

Run the application:

```powershell
python -m streamlit run app.py
```

## Suggested test scenarios

- Start a Customer session using a valid customer ID.
- Ask to view recent bookings and verify customer-scoped evidence.
- Start a Support Agent session with no customer ID and ask an operational question.
- Ask about cancellation rules and applicable policy versions.
- Test late-return and low-battery penalty questions against policy evidence.
- Ask a question whose answer is absent from the evidence.
- Test requests for unsupported refund guarantees.
- Start a new conversation and verify the previous chat is cleared.
- Verify that invalid customer IDs are rejected.

## Privacy and evidence grounding

Database queries use parameterized SQL and customer-scoped retrieval for Customer sessions. The LLM is instructed to rely on supplied evidence, avoid inventing policy terms or amounts, and acknowledge missing information.

Policy retrieval uses keyword-overlap ranking; it is not embedding-based vector search. Prompt instructions and evidence display reduce risk but do not guarantee that every answer is correct.

## Limitations

- Retrieval quality depends on question wording and available evidence.
- Scanned-document transcriptions require verification against original documents.
- Generated answers can contain errors and should be validated for consequential decisions.
- Role selection and customer ID validation are a hackathon demonstration, not production-grade authentication. Support Agent permissions require further independent security testing.
- The application is not established as a production-ready customer support system.

## Data handling

Do not commit API keys, passwords, customer data, or other secrets. Share the challenge database, policy documents, and organizer-provided materials only when the hackathon rules explicitly permit redistribution.

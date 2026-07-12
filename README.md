# Brand Guardian AI — Video Compliance Auditing Pipeline

An automated pipeline that audits video content (starting with YouTube videos) against brand/regulatory compliance guidelines. Built with **LangGraph** for agentic orchestration, **Azure AI Search** for retrieval of compliance rules, and **Groq-hosted LLMs** for fast reasoning over extracted claims.

## Overview

Given a video URL, the pipeline:

1. **Ingests** the video (YouTube → Azure)
2. **Indexes** it — extracting speech (Speech-to-Text) and on-screen text (OCR)
3. **Retrieves** the relevant compliance rules (e.g. rules around "Claims") from Azure AI Search
4. **Reasons** over the extracted claims against those rules using an LLM
5. Produces a structured **compliance audit report** — pass/fail status, violations by severity/category, and a natural-language summary

The workflow is orchestrated as a LangGraph graph (`START → Indexer → Auditor → END`), invoked from `main.py`.

## Tech Stack

| Component            | Technology                                  |
|-----------------------|----------------------------------------------|
| Orchestration          | LangGraph, LangChain                       |
| LLM Inference           | Groq (via `langchain-groq`) |
| Embeddings              | HuggingFace / Sentence-Transformers        |
| Retrieval / Indexing    | Azure AI Search (`azure-search-documents`) |
| Cloud / Auth             | Azure Identity, Azure Monitor (OpenTelemetry), Azure Storage Blob |
| Video ingestion           | `yt-dlp`                                  |
| API                        | FastAPI, Uvicorn                          |
| Language                          | Python 3.12+                        |

## Project Structure

```
compliance_azure/
├── backend/               # Core application logic
│   └── src/
│       └── graph/
│           └── workflow.py   # LangGraph workflow definition (exposes `app`)
├── main.py                # CLI entry point — runs a sample audit
├── pyproject.toml         # Project metadata & dependencies (uv-managed)
├── requirement.txt        # Pip requirements
├── uv.lock                # uv lockfile
├── .python-version        # Python version pin
└── README.md
```

## Getting Started

### Prerequisites

- Python 3.12+
- [uv](https://docs.astral.sh/uv/) (recommended) or pip
- Azure subscription with **AI Search** (and any other Azure services used, e.g. Storage) provisioned
- Groq API key
- (Optional) OpenAI-compatible API key if using `langchain-openai`

### Installation

Using `uv` (recommended, matches the included lockfile):

```bash
git clone https://github.com/Shaurya23102/compliance_azure.git
cd compliance_azure
uv sync
```

Or with pip:

```bash
git clone https://github.com/Shaurya23102/compliance_azure.git
cd compliance_azure
pip install -r requirement.txt
```

### Environment Variables

Create a `.env` file in the project root with the credentials your workflow needs, for example:

```env
GROQ_API_KEY=your_groq_api_key
OPENAI_API_KEY=your_openai_api_key

AZURE_SEARCH_ENDPOINT=your_search_endpoint
AZURE_SEARCH_API_KEY=your_search_key
AZURE_SEARCH_INDEX_NAME=your_index_name

AZURE_STORAGE_CONNECTION_STRING=your_storage_connection_string

DATABASE_URL=postgresql://user:password@host:port/dbname
REDIS_URL=redis://localhost:6379
```

> Adjust the variable names to match what's actually read inside `backend/src/`.

### Usage

Run the sample CLI audit (defined in `main.py`):

```bash
python main.py
```

This will:
1. Generate a session ID and build an initial audit request (currently a sample YouTube URL is hardcoded in `main.py`)
2. Invoke the LangGraph workflow (`Indexer → Auditor`)
3. Print a compliance audit report to the console — video ID, pass/fail status, violations by severity/category, and a final summary

To audit a different video, edit the `video_url` field inside `run_cli_simulation()` in `main.py` (or wire this up to accept a CLI argument / FastAPI endpoint).

## Roadmap

- [ ] Accept video URL as a CLI argument / API parameter instead of hardcoding it
- [ ] Expose the workflow via the included FastAPI app
- [ ] Streamlit dashboard for browsing audit reports
- [ ] Support batch video processing
- [ ] Configurable/extensible compliance rule sets

## Contributing

Contributions, issues, and feature requests are welcome. Feel free to open a PR or issue.

## Author

**Shaurya** — B.Tech Data Science & AI, IIIT Naya Raipur

# Compliance Azure — Video Compliance Auditing Pipeline

An automated pipeline for auditing video content against compliance guidelines, built with **LangGraph** for orchestration, **Azure Video Indexer** for video understanding, **Azure AI Search** for retrieval, and **Groq-hosted LLMs** for fast inference.

## Overview

This project ingests video content, extracts structured insights (transcripts, scenes, visual/audio labels, OCR text, etc.) using Azure Video Indexer, indexes the extracted data in Azure AI Search, and runs it through a LangGraph agentic workflow that checks the content against a defined set of compliance rules — flagging violations and generating an audit report.

## Features

- 🎥 **Video ingestion & analysis** via Azure Video Indexer (transcripts, scenes, labels, OCR, keyframes)
- 🔍 **Semantic + hybrid search** over extracted video insights using Azure AI Search
- 🧠 **Agentic compliance auditing** orchestrated with LangGraph, running rule-based and LLM-based checks
- ⚡ **Fast LLM inference** via Groq-hosted models for low-latency reasoning steps
- 📄 **Automated audit reports** summarizing compliance status, flagged segments, and reasoning

## Architecture

```
Video Input
    │
    ▼
Azure Video Indexer  ──►  Extracted Insights (transcript, scenes, labels, OCR)
    │
    ▼
Azure AI Search Index  ──►  Retrieval layer for relevant compliance context
    │
    ▼
LangGraph Agent Pipeline
    ├── Query/Segment Router
    ├── Compliance Rule Checker (Groq LLM)
    ├── Evidence Retrieval (Azure AI Search)
    └── Verdict & Report Generator
    │
    ▼
Compliance Audit Report
```

## Tech Stack

| Component            | Technology                          |
|-----------------------|--------------------------------------|
| Orchestration          | LangGraph                          |
| Video Understanding    | Azure Video Indexer                |
| Retrieval / Indexing   | Azure AI Search                    |
| LLM Inference          | Groq-hosted LLMs                   |
| Language                | Python                             |

## Getting Started

### Prerequisites

- Python 3.10+
- Azure subscription with **Video Indexer** and **AI Search** resources provisioned
- Groq API key

### Installation

```bash
git clone https://github.com/Shaurya23102/compliance_azure.git
cd compliance_azure
pip install -r requirements.txt
```

### Environment Variables

Create a `.env` file in the project root:

```env
AZURE_VIDEO_INDEXER_ACCOUNT_ID=your_account_id
AZURE_VIDEO_INDEXER_LOCATION=your_region
AZURE_VIDEO_INDEXER_API_KEY=your_api_key

AZURE_AI_SEARCH_ENDPOINT=your_search_endpoint
AZURE_AI_SEARCH_API_KEY=your_search_key
AZURE_AI_SEARCH_INDEX_NAME=your_index_name

GROQ_API_KEY=your_groq_api_key
```

### Usage

```bash
python main.py --video-path path/to/video.mp4
```

This will:
1. Upload and index the video via Azure Video Indexer
2. Push extracted insights into Azure AI Search
3. Run the LangGraph compliance auditing pipeline
4. Output a compliance audit report

## Project Structure

```
compliance_azure/
├── main.py                 # Entry point
├── pipeline/                # LangGraph pipeline definitions
├── indexer/                  # Azure Video Indexer integration
├── search/                   # Azure AI Search integration
├── agents/                    # Compliance-checking agent logic
├── requirements.txt
└── README.md
```

## Roadmap

- [ ] Support batch video processing
- [ ] Configurable compliance rule sets
- [ ] Web dashboard for audit report visualization
- [ ] Multi-model LLM fallback support

## Contributing

Contributions, issues, and feature requests are welcome. Feel free to open a PR or issue.

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

## Author

**Shaurya** — B.Tech Data Science & AI, IIIT Naya Raipur

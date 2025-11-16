# Research Assistant

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![PyPI version](https://badge.fury.io/py/research-assistant-llm.svg)](https://badge.fury.io/py/research-assistant-llm)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

An intelligent pipeline for processing research papers using LLMs (Ollama or Gemini) with **dynamic LLM-driven category generation**, accurate PDF parsing, metadata extraction, multi-category relevance scoring, deduplication, and automated summarization.

## Features

- **🤖 Dynamic LLM-Driven Taxonomy**: LLM generates categories from your research topic (no hardcoded categories!)
- **📊 Multi-Category Scoring**: Papers scored across ALL categories simultaneously for best-fit placement
- **🎯 Flexible LLM Support**: Use local Ollama models or Google Gemini API
- **🔧 Generic & Configurable**: Runtime topic and directory configuration (no hardcoding)
- **📄 Accurate PDF Parsing**: PyMuPDF + OCR fallback (ocrmypdf + Tesseract)
- **🔍 LLM-Based Metadata Extraction**: Extract titles, authors, abstracts, years using local or cloud LLMs
- **🔄 Smart Deduplication**: Exact (hash-based) and near-duplicate (MinHash-based) detection
- **📝 Topic-Focused Summaries**: Per-paper summaries with "how this helps your research"
- **💾 Resumable**: SQLite cache for embeddings and OCR outputs, index-based resume logic
- **📤 Multiple Outputs**: JSONL master index + CSV spreadsheet + Markdown summaries per category
- **⏱️ Rate Limiting**: Smart Gemini API rate limiting (10 RPM, 500 RPD) with warnings and interactive prompts
- **✅ Comprehensive Testing**: 220+ unit and integration tests with 77% coverage

## Pipeline Flow (8 Passes)

```mermaid
graph TD
    A[📁 Input: PDF Directory + Topic] --> B[🤖 PASS 1: LLM Taxonomy Generation]
    B -->|Generate categories from topic ONLY| C[� PASS 2: Inventory PDFs]
    C -->|Discover all PDFs| D[🔍 PASS 3: Metadata + Classification]
    D -->|Extract metadata + Multi-category scoring| E{Readable?}
    E -->|No| F[� Move to need_human_element/]
    E -->|Yes| G{Topic Relevance?}
    G -->|< threshold| H[� Move to quarantined/]
    G -->|>= threshold| I[📁 PASS 4: Move to Best Category]
    I -->|Highest scoring category| J[🔄 PASS 5: Deduplication]
    J -->|MinHash LSH| K{Duplicate?}
    K -->|Yes| L[� Move to repeated/]
    K -->|No| M[📝 PASS 6: Update Manifests]
    M --> N[✍️ PASS 7: LLM Summarization]
    N -->|Topic-focused summaries| O[💾 PASS 8: Generate Index]
    O --> P[📊 index.csv]
    O --> Q[📋 index.jsonl]
    O --> R[📝 summaries/*.md]
    O --> S[📜 manifests/*.json]
    O --> T[🗂️ categories.json]
    
    style B fill:#e1f5ff
    style D fill:#e1f5ff
    style N fill:#e1f5ff
    style F fill:#ffe1e1
    style H fill:#ffe1e1
    style L fill:#ffe1e1
    style P fill:#e1ffe1
    style Q fill:#e1ffe1
    style R fill:#e1ffe1
    style S fill:#e1ffe1
    style T fill:#e1ffe1
```

## Architecture

```
research_assistant/
├── cli.py                  # Main CLI entry point (8-pass pipeline)
├── config.py               # Configuration and settings
├── core/
│   ├── taxonomy.py         # LLM-based category generation from topic
│   ├── inventory.py        # Directory traversal and PDF discovery
│   ├── parser.py           # PDF text extraction (PyMuPDF + OCR)
│   ├── metadata.py         # LLM metadata extraction + multi-category scoring
│   ├── dedup.py            # MinHash near-duplicate detection
│   ├── embeddings.py       # Ollama embedding generation
│   ├── summarizer.py       # Topic-focused summary generation
│   ├── mover.py            # File moving with dynamic folder creation
│   ├── manifest.py         # Category manifest tracking
│   └── outputs.py          # JSONL, CSV, and Markdown generation
├── utils/
│   ├── cache_manager.py    # SQLite-based caching
│   ├── llm_provider.py     # Unified Ollama/Gemini interface
│   ├── gemini_client.py    # Google Gemini API client
│   ├── hash.py             # Content hashing utilities
│   └── text.py             # Text normalization and processing
└── tests/                  # 100+ unit and integration tests
```

## Prerequisites

- **Python 3.12+**
- **LLM Provider** (choose one or both):
  - **Ollama** (local, free) with models:
    - `deepseek-r1:8b` (metadata extraction & classification)
    - `nomic-embed-text` (embeddings)
  - **Google Gemini API** (cloud, requires API key):
    - Set `GEMINI_API_KEY` environment variable
- **Tesseract** (for OCR): `brew install tesseract` (macOS) or `apt-get install tesseract-ocr` (Linux)

## Installation

### From PyPI (Recommended)

```bash
# Install from PyPI
pip install research-assistant-llm

# Run interactive setup wizard (guides you through Ollama/Gemini setup)
research-assistant setup

# Or manual setup:
# Option 1: Use Ollama (local, free)
ollama pull deepseek-r1:8b
ollama pull nomic-embed-text

# Option 2: Use Gemini API (cloud-based)
export GEMINI_API_KEY="your_api_key_here"
```

### From Source (Development)

```bash
# Clone repository
git clone https://github.com/rexmirak/research_assistant.git
cd research_assistant

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install in editable mode
pip install -e .

# Install development dependencies
pip install -e ".[dev]"
```

## API Key Setup

### Gemini API (Cloud)

**Option 1: Environment Variable** (Recommended for CI/CD)
```bash
export GEMINI_API_KEY="your_api_key_here"
research-assistant process --llm-provider gemini --root-dir ./papers --topic "..."
```

**Option 2: .env File** (Convenient for local development)
```bash
# Create .env in your working directory
echo "GEMINI_API_KEY=your_api_key_here" > .env
research-assistant process --llm-provider gemini --root-dir ./papers --topic "..."
```

**Option 3: Config File**
```yaml
# config.yaml
gemini:
  api_key: "${GEMINI_API_KEY}"  # References environment variable
  # OR
  api_key: "your_api_key_here"  # Direct (not recommended for version control)
```

```bash
research-assistant process --config-file config.yaml --root-dir ./papers --topic "..."
```

**Get your Gemini API key**: https://aistudio.google.com/app/apikey

### Ollama (Local)

No API key needed! Just install Ollama and pull models:
```bash
# Install from https://ollama.com/download
ollama pull deepseek-r1:8b
ollama pull nomic-embed-text

research-assistant process --llm-provider ollama --root-dir ./papers --topic "..."
```

## Quick Start

```bash
# View help
research-assistant --help
research-assistant process --help

# Basic usage with Gemini (recommended)
research-assistant process \
  --root-dir /path/to/papers \
  --topic "Prompt Injection Attacks in Large Language Models" \
  --llm-provider gemini \
  --workers 2

# With Ollama (local, requires models installed)
research-assistant process \
  --root-dir /path/to/papers \
  --topic "Your research topic" \
  --llm-provider ollama \
  --workers 2

# Custom topic relevance threshold (default: 5/10)
research-assistant process \
  --root-dir /path/to/papers \
  --topic "Your research topic" \
  --min-topic-relevance 7

# Resume from interrupted run (skips analyzed papers)
research-assistant process \
  --root-dir /path/to/papers \
  --topic "Your research topic" \
  --resume

# Force regenerate categories (ignore cached taxonomy)
research-assistant process \
  --root-dir /path/to/papers \
  --topic "Your research topic" \
  --force-regenerate-categories

# Dry-run (no file moves)
research-assistant process \
  --root-dir /path/to/papers \
  --topic "Your research topic" \
  --dry-run
```

## Configuration

Runtime configuration via CLI flags or `config.yaml`:

```yaml
# config.yaml (optional)
llm_provider: gemini  # or 'ollama'

# Scoring thresholds
scoring:
  min_topic_relevance: 5  # Papers below this go to quarantined/ (1-10 scale)

# Deduplication
dedup:
  similarity_threshold: 0.95
  use_minhash: true
  num_perm: 128

# LLM providers
ollama:
  summarize_model: "deepseek-r1:8b"
  classify_model: "deepseek-r1:8b"
  embed_model: "nomic-embed-text"
  temperature: 0.1
  base_url: "http://localhost:11434"

gemini:
  api_key: null  # Set via GEMINI_API_KEY environment variable
  temperature: 0.1

# Rate limiting (Gemini API)
rate_limit:
  enabled: true
  rpm_limit: 10   # Requests per minute (Gemini free tier)
  rpd_limit: 500  # Requests per day (Gemini free tier)
  # Warnings at 50% (250 RPD) and 75% (375 RPD)
  # Interactive prompt at daily limit with options:
  #   1. Pause and resume tomorrow
  #   2. Switch to Ollama (local)
  #   3. Continue anyway (risky)

# Metadata enrichment
crossref:
  enabled: true
  email: "your.email@domain.com"  # Polite pool (optional)

# File organization
move:
  enabled: true
  track_manifest: true
  create_symlinks: false

# Processing
processing:
  workers: 2  # Parallel workers (recommend 2 for API rate limits)
  batch_size: 32
```

## Rate Limiting (Gemini API)

**Automatic rate limiting** prevents API failures and quota exhaustion:

- **RPM Tracking**: Enforces 10 requests per minute (Gemini free tier)
  - Automatically adds delays between requests to stay under limit
  - Thread-safe implementation for parallel workers

- **RPD Tracking**: Monitors 500 requests per day limit
  - Warning at 50% usage (250 requests)
  - Warning at 75% usage (375 requests)
  - Interactive prompt at limit with options:
    1. **Pause**: Stop processing, resume tomorrow (preserves progress)
    2. **Switch to Ollama**: Continue with local LLM (no API costs)
    3. **Continue anyway**: Risk API errors (not recommended)

- **Persistent State**: Tracks usage across runs in `cache/rate_limit_state.json`
- **Disable**: Set `rate_limit.enabled: false` in config to disable

**Example output:**
```
⚠️  WARNING: 75% of daily Gemini quota used (375/500 requests)
Consider switching to Ollama to preserve remaining quota.

🛑 Daily Gemini API limit reached (500/500 requests)
Options:
  1. Pause processing and resume tomorrow
  2. Switch to Ollama (local, no API costs)
  3. Continue anyway (may fail)
```

## Dynamic Category Generation

**How it works**:

1. **LLM generates categories from topic ONLY** (no papers analyzed yet)
   - Example topic: "Prompt Injection Attacks in Large Language Models"
   - LLM generates 10-15 relevant categories with definitions
   - Cached in `outputs/categories.json` and `cache/categories.json`

2. **Multi-category scoring** for each paper:
   - Paper scored against ALL categories simultaneously (1-10 scale)
   - Returns: `topic_relevance`, `category_scores` dict, `reasoning`
   - Paper placed in highest-scoring category

3. **Topic relevance filtering**:
   - Papers with `topic_relevance < threshold` → `quarantined/`
   - Configurable via `--min-topic-relevance` (default: 5/10)

**Example Categories Generated**:
```json
{
  "attack_vectors": "Papers describing methods to perform prompt injection...",
  "defense_mechanisms": "Papers proposing techniques to defend against...",
  "detection_methods": "Papers focusing on identifying attacks...",
  "robustness_evaluation": "Papers developing metrics and benchmarks..."
}
```

## Manifest System & Resume Logic

**Manifest Structure** (per category):
- Tracks all papers in this category
- Stores classification reasoning and scores
- Enables resume functionality

**Manifest Entry**:
```json
{
  "paper_id": "abc123def456...",
  "title": "Defending Against Prompt Injection Attacks",
  "path": "defense_mechanisms/smith2023.pdf",
  "content_hash": "sha256:...",
  "classification_reasoning": "Paper focuses on input validation...",
  "relevance_score": 9,
  "topic_relevance": 8,
  "analyzed": true
}
```

**Resume Logic**:
- Checks `index.jsonl` for papers with `analyzed: true`
- Skips re-processing, loads from cache
- More efficient than re-running entire pipeline

## Output Structure

```
outputs/
├── categories.json          # LLM-generated taxonomy with definitions
├── index.jsonl              # Full machine-readable index
├── index.csv                # Spreadsheet with all metadata
├── summaries/
│   ├── attack_vectors.md    # Dynamic category names
│   ├── defense_mechanisms.md
│   ├── quarantined.md
│   └── ...
├── logs/
│   └── pipeline_YYYYMMDD_HHMMSS.log  # Detailed execution log
└── manifests/
    ├── attack_vectors.manifest.json  # Dynamic categories
    ├── defense_mechanisms.manifest.json
    ├── quarantined.manifest.json
    ├── repeated.manifest.json
    └── need_human_element.manifest.json
```

## Index Fields (JSONL/CSV)

- `paper_id`: Unique identifier (content hash)
- `title`, `authors`, `year`, `venue`, `doi`, `bibtex`
- `category`: Final category (best-fit from LLM scoring)
- `topic_relevance`: 1-10 relevance to research topic
- `category_scores`: JSON dict with scores for ALL categories
- `reasoning`: LLM explanation for categorization
- `duplicate_of`: Paper ID if duplicate
- `is_duplicate`: Boolean flag
- `path`: Current file path
- `summary_file`: Link to markdown summary
- `analyzed`: Boolean (true when processing complete)

## Advanced Usage

### Custom topic relevance threshold
```bash
# Stricter filtering (only highly relevant papers)
research-assistant process \
  --root-dir ./papers \
  --topic "..." \
  --min-topic-relevance 7

# More permissive (include more papers)
research-assistant process \
  --root-dir ./papers \
  --topic "..." \
  --min-topic-relevance 3
```

### Working with cached categories
```bash
# Use cached taxonomy (fast)
research-assistant process --root-dir ./papers --topic "..." --resume

# Force regenerate taxonomy (if topic changed)
research-assistant process \
  --root-dir ./papers \
  --topic "..." \
  --force-regenerate-categories
```

### Parallel processing
```bash
# More workers (caution: rate limiter adds delays)
research-assistant process \
  --root-dir ./papers \
  --topic "..." \
  --workers 4

# Recommended for Gemini free tier (rate limiter enforces 10 RPM)
research-assistant process \
  --root-dir ./papers \
  --topic "..." \
  --workers 2
```

## Troubleshooting

### OCR failing
```bash
# Verify Tesseract installation
tesseract --version

# Install additional language packs if needed
brew install tesseract-lang
```

### Ollama connection issues
```bash
# Check Ollama is running
ollama list

# Restart Ollama service
brew services restart ollama
```

## Performance Tips

- **Parallel processing**: Set `--workers 2-4` for multiprocessing (rate limiter handles coordination)
- **Rate limit awareness**: Gemini free tier enforces 10 RPM (automatically managed)
- **Cache warming**: Run inventory + parsing first, then scoring/summarization
- **Selective OCR**: Skip OCR for born-digital PDFs (auto-detected)
- **Batch embeddings**: Automatically batched in groups of 64
- **Resume capability**: Use `--resume` to skip already-analyzed papers

## Testing & Quality

```bash
# Run full test suite
pytest

# Run with coverage
pytest --cov=core --cov=utils --cov-report=html

# Run specific test file
pytest tests/test_metadata.py -v

# Type checking
mypy core/ utils/ --explicit-package-bases --ignore-missing-imports

# Linting
flake8 core/ utils/ tests/

# Security scanning
pip-audit --requirement requirements.txt
bandit -r core/ utils/ -ll
```

**CI/CD**: GitHub Actions runs all quality checks on Python 3.12 & 3.13
- ✅ Linting (flake8)
- ✅ Type checking (mypy)
- ✅ Security scanning (pip-audit, bandit)
- ✅ Tests (pytest)
- ✅ Documentation checks
- ✅ Build verification

## License

MIT

````

# Automated Analyst Report Pipeline

A five-component autonomous pipeline that ingests supply chain data,
retrieves relevant financial commentary via vector search, generates
a professional analyst report using Llama 3.1 70B, and emails it as
a formatted PDF — on a schedule, with zero human input.

## Architecture

Query → Parser (Groq) → SQL Retrieval (SQLite) → Vector Retrieval (FAISS)
→ Context Assembly → Report Generation (Groq) → PDF (ReportLab) → Email

## Sample Output

See `outputs/reports/sample_report.pdf` for a real generated report.

Key findings from the supply chain dataset:
- Haircare: highest defect rate (2.48%), Cosmetics: lowest (1.92%)
- 36 SKUs failed inspection; SKU42 at 4.94% defect rate
- Carrier C: best revenue-to-cost ratio ($1,138.56 per $1 shipped)
- SKU34: critical stockout risk — 1 unit in stock, 26-day lead time

## Setup

### Prerequisites
- Python 3.10+
- Groq API key (free): https://console.groq.com
- Kaggle account + API token: https://www.kaggle.com
- Gmail account with App Password enabled

### Installation

git clone https://github.com/YOUR_USERNAME/analyst-report-pipeline
cd analyst-report-pipeline
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

### Configuration

cp .env.example .env
# Edit .env and add your GROQ_API_KEY and EMAIL_PASSWORD

### Data

Download the datasets via Kaggle CLI:

kaggle datasets download -d harshsingh2209/supply-chain-analysis \
  -p data/structured --unzip

kaggle datasets download -d ankurzing/sentiment-analysis-for-financial-news \
  -p data/unstructured --unzip

### Run

Open analyst_pipeline.ipynb in Jupyter and run all cells in order.
The scheduler in the final cell runs the pipeline automatically.

## Stack

| Component | Technology |
|---|---|
| LLM | Groq / Llama 3.1 70B |
| Vector Search | FAISS + Sentence Transformers |
| Structured Data | SQLite + Pandas |
| PDF Generation | ReportLab |
| Scheduling | APScheduler |
| Email | Gmail SMTP |

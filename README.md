# Policy RAG Assistant: Hybrid-Retrieval Compliance Engine

A production-grade, work-in-progress Retrieval-Augmented Generation (RAG) assistant designed to make dense international development policy frameworks accessible to non-specialist users. This engine pairs **Claude 3.5 Sonnet** with a dual-engine **Hybrid Retrieval (BM25 + ChromaDB)** framework to enforce absolute grounding, completely eliminating speculative model hallucinations in high-stakes compliance environments.

---

## 🚀 The Core Problem & Solution

**The Problem:** Standalone Large Language Models (LLMs) are highly confident autocomplete engines. When querying detailed development policies, they routinely generate plausible-sounding but completely fabricated clauses (hallucinations), creating massive compliance and safety risks.

**The Solution:** This project transforms the query loop from a "closed-book" memory game into an **"open-book" verified audit trail**. By binding an isolated LLM to a hybrid local knowledge base, the tool guarantees that answers are extracted directly from authenticated reference chunks or the system fails safely.

---

## 🛠️ System Architecture & Hybrid Search Pipeline

The tool implements an Advanced RAG paradigm using **Reciprocal Rank Fusion (RRF)** to combine two distinct search methods:

1. **Keyword Search (BM25)**: Targets exact matches, including specific clause numbers, legal acronyms, and direct section strings (e.g., *"Section 4.1.a"*).
2. **Semantic Search (ChromaDB Vector)**: Evaluates vector dot-products to understand abstract concepts, context, and structural synonyms (e.g., matching *"data privacy"* with *"information safety protocols"*).

```text
User Query ──> [ Ingest & Embed Data ]
                     │
                     ├──> BM25 Engine (Keyword Match) ──┐
                     │                                   ├──> [ Reciprocal Rank Fusion ] ──> Grounded Context ──> [ Claude 3.5 Sonnet ] ──> Verified Audit Trail
                     └──> ChromaDB (Semantic Vector) ───┘
```

### Key Engineering Features
* **Fact-Locked System Prompts**: Structural directives force Claude to return a standard verification failure code rather than guessing if proof is missing.
* **Deterministic Configuration**: Run parameters fix model `temperature` strictly to `0.0` for factual consistency.
* **Traceable Citations**: System automatically maps and parses source file metadata directly into the frontend interface.

---

## 📁 Repository Structure

```text
policy-rag-assistant/
│
├── documents/              # Staging directory for policy source PDFs
│
├── db/                     # Local persistent ChromaDB vector records
│
├── requirements.txt        # Python deployment manifests
├── app.py                  # Live Streamlit Web User Interface Dashboard
└── pipeline.py             # Dual-engine search architecture & RAG orchestration
```

---

## ⚡ Quick Start & Deployment

### 1. Clone & Set Up Isolated Sandbox Environment
```bash
git clone https://github.com
cd policy-rag-assistant
python3 -m venv compliance_env
source compliance_env/bin/activate
```

### 2. Install Dependency Framework Matrices
```bash
pip install -r requirements.txt
```

### 3. Inject Secure System Environment API Credentials
```bash
export ANTHROPIC_API_KEY="your-anthropic-api-key-here"
```

### 4. Fire Up the Dashboard Interface
```bash
streamlit run app.py
```

---

## 🔬 Rigorous Testing & Evaluation Methodology

To evaluate the reliability of your local node, run these testing procedures:

1. **Baseline Ingestion Validation**: Drop a standard multi-page UN or World Bank PDF policy into `documents/`. Use the sidebar to compile the index, and ask for specific section data to confirm chunk continuity.
2. **The Hallucination Trap**: Ask a highly specific question about a country or clause *completely missing* from your files. The engine should output:  
   `"Factual verification failed: Data missing from references."`  
   This proves the strict context boundaries are holding successfully.

---

## 🛠️ Roadmap & Work-in-Progress Focus
* [x] Core Data Ingestion Pipeline (PyPDF)
* [x] Hybrid Search Integration (BM25 + Vector RRF)
* [x] Deterministic Claude 3.5 Grounding & Metadata Citation
* [ ] Programmatic Evaluation Frameworks (Integrating **Ragas / TruLens** metrics)
* [ ] Local Deployment Transition (Migrating from cloud API to a fully local, open-source model using Ollama)

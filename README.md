# Policy RAG Assistant: Decoupled Streaming Compliance Engine 📖🚀

A production-grade, full-stack Retrieval-Augmented Generation (RAG) tool designed to make dense international development policy frameworks accessible to non-specialist users. This architecture decouples concerns completely by deploying a **FastAPI backend orchestration service** powered by **Google Gemini 2.5 Pro** and a **ChromaDB + BM25 Hybrid Search Mesh**, communicating with a high-performance **TailwindCSS/JS frontend** using Server-Sent Events (SSE) for token-by-token real-time streaming.

---

## 🛠️ System Architecture Diagram

```text
  [ Client UI Interface ] (TailwindCSS / JS)
            │                  ▲
    POST File / Query     Server-Sent Events (Token Streaming)
            ▼                  │
  [ FastAPI Backend Engine ] ──┼──────────────────────┐
            │                  │                      │
            ▼                  │                      ▼
    BM25 Keyword Index         │             ChromaDB Vector Index
    (Exact Clause Tracking)    │             (Semantic Meaning Match)
            │                  │                      │
            └───────────> [ Reciprocal ] <────────────┘
                          [ Rank Fusion]
                               │
                               ▼
                        [ Gemini 2.5 Pro ]
```

### Key Technical Decisions
* **Decoupled Architecture**: Splitting the frontend client from the engine ensures the heavy compute of document embedding calculations doesn't block UI interactions.
* **Hybrid Search Formulation**: Intersects traditional lexical frequency rankings (BM25) with dense semantic spaces (`text-embedding-004`) using a 50/50 rank fusion blend to track specific section indices accurately.
* **Deterministic Grounding**: Pinning the model execution temperature to `0.0` and implementing fact-locked prompts removes creative guessing, forcing the application to report gaps safely.

---

## 🚀 Deployment Instructions

### 1. Set Up the Asynchronous FastAPI Server Layer
```bash
cd backend
python3 -m venv compliance_env
source compliance_env/bin/activate
pip install -r requirements.txt
export GOOGLE_API_KEY="AIzaSyYourActualGoogleStudioKeyHere"
python main.py
```
*The service will start local operations at `http://127.0.0.1:8000`.*

### 2. Spin Up the Frontend UI Framework
In a separate terminal instance, serve the client code to prevent local security file blocks:
```bash
cd frontend
python3 -m http.server 3000
```
*Open your web browser and navigate to `http://localhost:3000`.*

---

## 🔬 Evaluation & Reliability Tests

To evaluate system functionality, run this check:
1. **The Failsafe Test**: Ask the system about information completely unrelated to your uploaded documents. The application should safely output:  
   `"Factual verification failed: Data missing from references."`  
   This confirms that the context guardrails are running correctly.

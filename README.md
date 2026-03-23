# 🤖 Technical Intro to Generative AI — Lab Repository

> **Build a RAG pipeline or an Agentic workflow from scratch.**
> This repo gives you the code scaffolds, lab guides, and environment setup for a one-day, hands-on GenAI course designed for engineers who want to move from "I've heard of this" to "I built this."

---

## 💡 The "Why" — A Real-World Analogy

### Track A builds on **RAG (Retrieval-Augmented Generation)**

Imagine you're a brilliant consultant who has read every book ever written — but your memory is frozen in time. When a client asks a question about *their* specific internal documents, you'd be making things up. That's an LLM without RAG.

**RAG is the research assistant** sitting beside that consultant. Before the consultant speaks, the assistant sprints to the filing cabinet, pulls the three most relevant pages, and hands them over. Now the consultant answers using *real, grounded evidence* — not fabricated memory.

In code terms: your documents are chunked → converted into numerical "meaning vectors" → stored in a vector database → retrieved by similarity when a question arrives → passed as context to the LLM, which generates a grounded answer.

### Track B builds on **Agentic AI (Plan → Act → Observe → Refine)**

Think of a junior analyst who doesn't just answer questions but can *use tools* — run a search, query a database, do a calculation — and decide which tool to pick based on what the task needs. An AI Agent is that analyst: it plans, acts, observes the result, and refines its next step. It doesn't just generate text; it executes a loop until the job is done.

---

## 🚀 Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/your-org/genai-tech-intro.git
cd genai-tech-intro

# 2. Create and activate a virtual environment
python3 -m venv genai_env
source genai_env/bin/activate        # macOS / Linux
# genai_env\Scripts\activate         # Windows

# 3. Install all dependencies
pip install -r requirements.txt

# 4. Set up your API credentials
cp .env.example .env
# Open .env in your editor and add your OPENAI_API_KEY

# 5. Verify your setup
python scripts/verify_env.py
```

> 📖 **Before you run anything**, read [`PREREQUISITES.md`](./PREREQUISITES.md) to make sure your API keys and Python version are in order.

### Choosing your track

| If you're building… | Go to… |
|---|---|
| A document Q&A / knowledge retrieval tool | `labs/track_a_rag/` |
| An autonomous agent with tools and a decision loop | `labs/track_b_agent/` |

Each lab folder contains its own `README.md` with step-by-step instructions.

---

## 🎯 Learning Objectives

By exploring and completing the labs in this repository, you will be able to:

**Foundations (both tracks)**
- Explain how a large language model generates a response, including tokenization, embeddings, and the attention mechanism
- Identify the key parameters that shape model behavior (temperature, top-p) and describe at least two failure modes — hallucination and context window overflow
- Write, debug, and evaluate structured prompts using zero-shot, few-shot, and Chain-of-Thought patterns

**Track A — RAG Workflow**
- Design a document corpus for retrieval quality, including chunking strategy and deduplication
- Build an end-to-end ingestion pipeline: load → normalize → chunk → embed → index with FAISS
- Construct a generation prompt that grounds LLM output in retrieved context, and measure retrieval precision against a self-defined evaluation set

**Track B — Agentic Pipeline**
- Implement a LangChain agent with the plan → act → observe → refine loop
- Define tools with precise descriptions and validated input/output contracts using Pydantic
- Observe how prompt design shapes agent behavior and identify where agentic patterns introduce risk

**Cross-cutting skills**
- Manage API credentials securely using `.env` files and `python-dotenv`
- Use logs and traces to debug prompt failures and retrieval issues at the source
- Articulate the gap between a technically working system and one that meets a professional deployment standard

---

## 📁 Repository Structure

```
genai-tech-intro/
├── README.md               ← You are here
├── PREREQUISITES.md        ← Start here before anything else
├── requirements.txt        ← All Python dependencies
├── .env.example            ← Template for your API keys
├── scripts/
│   └── verify_env.py       ← Sanity-check your setup
├── labs/
│   ├── track_a_rag/        ← RAG pipeline lab (Session 3, Track A)
│   │   ├── README.md
│   │   ├── 01_corpus_prep.py
│   │   ├── 02_chunking.py
│   │   ├── 03_embed_and_index.py
│   │   └── 04_retrieve_and_generate.py
│   └── track_b_agent/      ← Agentic pipeline lab (Session 3, Track B)
│       ├── README.md
│       ├── tools/
│       └── agent_loop.py
├── notebooks/
│   ├── session1_tokenization.ipynb
│   └── session2_prompt_engineering.ipynb
└── corpus/                 ← Drop your source documents here
```

---

## 🧭 Course Map

This repo supports a four-session, one-day course:

| Session | Topic | Relevant Files |
|---|---|---|
| Session 1 | LLM Architecture & Model Behavior | `notebooks/session1_tokenization.ipynb` |
| Session 2 | Prompt Engineering, RAG & Agentic Concepts | `notebooks/session2_prompt_engineering.ipynb` |
| Session 3A | RAG Workflow Capstone Lab | `labs/track_a_rag/` |
| Session 3B | Agentic Pipeline Capstone Lab | `labs/track_b_agent/` |
| Session 4 | Cross-track Debrief & Reflection | Discussion — no code files |

---

## 🙌 A Note to Students

**You are not expected to finish.** This course is designed so that a skilled participant goes deep and a less-experienced participant builds a solid foundation — and both outcomes are equally valid.

The lab document is your primary artifact. Leave today with a working foundation, your own annotations, and a concrete idea of how you'll apply this to a real problem in the next 30 days. That specific application — not a polished codebase — is the goal.

If something breaks, lean into it. The trace and the log are where the learning lives.

---

## 📄 License

This repository is for educational use. Please refer to your course facilitator for distribution and reuse guidelines.

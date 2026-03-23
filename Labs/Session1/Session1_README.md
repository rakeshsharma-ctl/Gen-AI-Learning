# Session 1 -- LLM Architecture and Model Behavior

**Technical Intro to Generative AI**
McKinsey Technical Professionals | TLU Mode | 90-110 minutes

---

## Overview

Session 1 establishes the shared technical foundation for the full one-day program. It is designed for Data Engineers, ML Engineers, Backend Developers, Cloud Engineers, and Technical Professionals who have not yet systematically incorporated Generative AI into their workflows.

By the end of this session every participant can explain how an LLM generates a response, name the key parameters that shape its behavior, and describe at least two failure modes and why they occur.

This session is a prerequisite for Sessions 2, 3, and 4. Participants who complete it share a common vocabulary and mental model that all subsequent content builds on.

---

## Session at a Glance

| Item | Detail |
|------|--------|
| Duration | 90-110 minutes |
| Learning Units | 22 (LU-S1-01 to LU-S1-22) |
| Mode | TLU -- Tailored Learning Unit |
| Audience | McKinsey technical professionals, all tracks |
| Position in program | First session -- shared foundation |

---

## Lesson Map

| Lesson | Title | LU IDs | Approach Types |
|--------|-------|--------|----------------|
| Lesson 1 | From Neural Networks to Transformers | LU-S1-01 to 04 | PR, CF, TP, HLA |
| Lesson 2 | Context, Throughput, and Model Selection | LU-S1-05 to 08 | PR, CF, TP, HLA |
| Lesson 3 | Attention Internals and Multi-Head Mechanisms | LU-S1-09 to 12 | PR, CF, TP, HLA |
| Lesson 4 | Parameters and Output Behavior | LU-S1-13 to 16 | PR, CF, TP, HLA |
| Lesson 5 | Failure Modes and Programmatic Inspection | LU-S1-17 to 20 | PR, CF, TP, HLA |
| Lab | Tokenization, Embeddings, and Parameter Tuning | LU-S1-21 to 22 | TP, TP |

**Approach Type Key**

| Code | Meaning |
|------|---------|
| PR | Problem Recognition |
| CF | Conceptual Framework |
| TP | Task Procedure |
| HLA | High-Leverage Application |

Each lesson follows the same four-unit arc: recognize a failure or limitation in current mental models, build the conceptual framework that explains it, follow the task procedure that operationalizes it, and apply it to a high-leverage decision in real technical work.

---

## Learning Objectives

After completing Session 1 a participant will be able to:

1. Explain why LLM outputs are stochastic and how that differs from classical ML model behavior.
2. Trace the token-embedding-attention-output path for a single inference request.
3. Predict integration failure types that arise from transformer architecture constraints: context window limits, tokenization errors, and throughput costs.
4. Select a foundation model for a specific use case by matching architecture properties to requirements.
5. Diagnose attention-related failures using controlled position variation experiments.
6. Set temperature, top-p, and penalty parameters deliberately based on output type and compliance requirements.
7. Build a structured logging and quality-check pipeline for LLM outputs.
8. Scope human review requirements for a deployment based on failure mode and consequence profile.

---

## Lesson Summaries

### Lesson 1 -- From Neural Networks to Transformers (LU-S1-01 to 04)

Addresses the three ways transformer-based LLMs behave differently from the neural network mental models most technical professionals already hold: stochastic outputs from sampling-based decoding, opaque reasoning paths distributed across billions of parameters, and unbounded task scope that produces unfamiliar failure modes.

Covers the full transformer architecture: tokenization, embeddings, positional encoding, the attention mechanism, stacked transformer blocks, and autoregressive output generation. Provides the task procedure for tracing a single inference request from raw text to generated token. Closes with three high-leverage applications of architecture knowledge: context window management, tokenization-driven failure diagnosis, and throughput estimation.

**Key concepts:** token, embedding, positional encoding, self-attention, transformer block, autoregressive generation, stochastic output, referential transparency failure.

---

### Lesson 2 -- Context, Throughput, and Model Selection (LU-S1-05 to 08)

Addresses the context window constraint failure pattern: documents that appear to fit the window but cause silent truncation, attention quality degradation in the middle of long inputs, and latency and cost anomalies that look like infrastructure problems but are architectural in origin.

Covers three architectural properties that determine deployability: context window size and the gap between stated maximum and practically reliable length, inference path and how autoregressive generation makes output length a latency lever, and throughput as the binding constraint for batch use cases. Provides a structured model selection procedure across GPT, Claude, Gemini, and Mistral. Closes with the data handling and compliance constraint as the first selection gate before any performance evaluation.

**Key concepts:** context window, effective context length, lost-in-the-middle effect, autoregressive latency, throughput, rate limit, closed-weight vs. open-weight models, data residency.

---

### Lesson 3 -- Attention Internals and Multi-Head Mechanisms (LU-S1-09 to 12)

Addresses misdiagnosed attention failures: a three-hour debugging session that should have been thirty minutes, caused by treating an attention distribution problem as a prompt language problem.

Covers the query-key-value mechanism, how attention scores are computed and how they form the attention matrix, and how multi-head attention runs independent attention computations in parallel to capture syntactic dependencies, coreference, and semantic similarity simultaneously. Provides the controlled variation experiment procedure for diagnosing attention dilution without direct access to attention weights. Closes with the two categories of structural capability: tasks where multi-head attention provides genuine leverage (distributed context integration) and tasks where it introduces failure risk (exact recall, character-level operations, middle-context retrieval).

**Key concepts:** query, key, value, attention matrix, softmax, multi-head attention, attention dilution, lost-in-the-middle, positional sensitivity.

---

### Lesson 4 -- Parameters and Output Behavior (LU-S1-13 to 16)

Addresses the default parameter failure pattern: format variability, JSON breakage, and output length inconsistency that appear to be prompt failures but are sampling parameter failures because the API was never explicitly configured.

Covers temperature as a logit scaling factor (not a style preference), top-p nucleus sampling and how it combines with temperature to control the token eligibility space, and frequency and presence penalties for repetition management. Provides a step-by-step parameter tuning procedure: define the compliance criterion first, sweep temperature in 0.1 increments, set top-p second, apply penalties only if repetition is observed. Closes with a risk-based sequencing framework: downstream-automated outputs first (temperature near 0.0), client-facing outputs second (0.1-0.3), internal reasoning outputs third (0.4-0.7).

**Key concepts:** temperature, logit, softmax, top-p, nucleus sampling, frequency penalty, presence penalty, compliance rate, referentially transparent.

---

### Lesson 5 -- Failure Modes and Programmatic Inspection (LU-S1-17 to 20)

Addresses the three failure modes that share a surface characteristic making them hard to catch: they look like correct outputs. Hallucinations present with the same confidence and formatting as accurate responses. Reasoning gap outputs show step-by-step logic that leads to a wrong answer. Overconfident outputs are calibrated toward sounding certain rather than communicating actual uncertainty.

Covers the mechanism behind each failure type: hallucination as a training distribution gap (the model generates citation-shaped output because the position calls for it), reasoning gaps as pattern reproduction rather than computation, and overconfident outputs as an artifact of human feedback training that penalizes hedging. Provides the structured logging and automated checking procedure: every call logged with prompt, response, parameters, token counts, and timestamp; automated checks for schema compliance, length bounds, and prohibited patterns; random sampling from production inputs rather than development test sets. Closes with the scoping framework for human review: highest burden on factual client-facing outputs, schema validation plus sampling for downstream-automated outputs, light developer review for internal reasoning steps.

**Key concepts:** hallucination, reasoning gap, overconfident output, training distribution, retrieval grounding, structured logging, automated quality check, human review scoping.

---

### Lab -- Tokenization, Embeddings, and Parameter Tuning (LU-S1-21 to 22)

Two hands-on exercises that build intuition for the concepts covered in Lessons 1 through 4.

Exercise 1 (LU-S1-21): tokenize a short text string, observe how domain-specific terms split into subword units, generate embeddings for sentence pairs, and compute cosine similarity to observe how the model's training distribution represents semantic relationships.

Exercise 2 (LU-S1-22): run the same prompt at five temperature values (0.0, 0.2, 0.5, 0.8, 1.2), generate three outputs per temperature, identify the compliance threshold, and test top-p combinations at the compliant temperature. Document the final parameter configuration with the compliance rate as evidence.

---

## Deliverables and Files

| File | Description |
|------|-------------|
| `McK_GenAI-Tech-Intro_Session1_LUs.docx` | Full learning unit content for all 22 LUs including facilitator notes and tailoring flags |
| `Session1_HandsOn_Lab_Guide.docx` | Printable lab guide with fillable tables, step-by-step instructions, and reflection prompts for all four lab exercises |
| `Session1_Lab_Notebook.ipynb` | Jupyter notebook for students with all setup code, exercise scaffolding, and open response cells to complete during the session |
| `Session1_Solutions_Notebook.ipynb` | Jupyter notebook with model answers for every exercise, fill-in, and reflection -- for instructor and self-study use |

---

## Prerequisites

Participants should arrive with:

- Python 3.9 or later installed
- Access to at least one LLM API (OpenAI, Anthropic, or equivalent approved provider)
- An API key set as an environment variable (`OPENAI_API_KEY` or equivalent)
- The following Python packages installed: `tiktoken`, `openai`, `numpy`

```bash
pip install tiktoken openai numpy
```

Participants do not need prior LLM experience. Familiarity with Python and at least one ML framework (TensorFlow, PyTorch, or scikit-learn) is assumed.

---

## Lab Quick-Start

```python
import os
from openai import OpenAI
import tiktoken
import numpy as np

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))
enc    = tiktoken.encoding_for_model("gpt-4o")

# Tokenize any string
tokens = enc.encode("The transformer attention mechanism scales quadratically.")
print(len(tokens), "tokens")
```

Full lab instructions are in `Session1_Lab_Notebook.ipynb`. Open the notebook and run the Global Setup cell before starting any lab section.

---

## Tailoring Notes

The session content was written for McKinsey technical professionals and preserves the following client-specific language intentionally:

- References to McKinsey systems, data handling policies, and approved provider lists appear in HLA units (LU-S1-04, S1-08, S1-12, S1-16, S1-20).
- Tool references (tiktoken, BertViz, transformer-lens) are preserved as TLU content in lab and diagnostic units.
- Model provider names (GPT, Claude, Gemini, Mistral) are preserved in the model selection unit (LU-S1-07).

Units marked with a TAILORING FLAG contain specific phrases that should be updated when delivering to a different organization or cohort. These flags are documented inline in `McK_GenAI-Tech-Intro_Session1_LUs.docx`.

---

## Connection to the Rest of the Program

Session 1 feeds directly into:

- **Session 2** -- Prompt Engineering, RAG Concepts, and Agentic Patterns. The architecture vocabulary from Session 1 (context window, tokenization, attention, temperature) is used throughout Session 2 to explain why specific prompt structures and retrieval patterns work.
- **Session 3 Track A** -- RAG Workflow. The embedding and cosine similarity concepts from the Session 1 lab underpin the vector indexing and retrieval layer built in Track A.
- **Session 3 Track B** -- Agentic Pipeline. The failure mode and programmatic inspection content from Lesson 5 directly informs the risk and reliability section of the agentic pipeline build.
- **Session 4** -- Cross-Track Debrief. The trust and human review framework introduced in LU-S1-20 is the conceptual anchor for the session-closing debrief question: what would you trust this to do in a real engagement, and what would you still want a human to review?

---

## Session Outcome

Every participant leaves Session 1 able to:

- Explain how an LLM generates a response without using the phrase "the AI thinks"
- Name the key architectural property behind each of the three common integration failure types they are most likely to encounter in their first production deployment
- Set sampling parameters deliberately for at least one real output type they work with
- Describe where in their current or upcoming work they would require human review of LLM output and why

---

*Technical Intro to Generative AI -- Session 1 | McKinsey Internal Training*

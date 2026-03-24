# Session 2 — Prompt Engineering, RAG Concepts & Agentic Patterns
### Technical Intro to Generative AI | McKinsey Internal Training

---

## Session at a Glance

| Attribute | Value |
|-----------|-------|
| **Duration** | 90-110 minutes |
| **Learning Units** | 22 (LU-S2-01 through LU-S2-22) |
| **Lessons** | 5 lessons + 1 lab |
| **Audience** | Data Engineers, ML Engineers, Backend Developers, Cloud Engineers, Technical Professionals |
| **Position in program** | Session 2 of 4 — builds directly on Session 1 (LLM architecture and model behavior) |
| **Lab deliverable** | Agent-aware prompt templates + Pydantic structured output validation |

---

## Lesson Map

| Unit | Title | LU IDs | Approach Types |
|------|-------|--------|----------------|
| Lesson 1 | Prompt Structure and Layering | LU-S2-01 to 04 | PR, CF, TP, HLA |
| Lesson 2 | Shot Patterns, Chain-of-Thought, and ReAct | LU-S2-05 to 08 | PR, CF, TP, HLA |
| Lesson 3 | Debugging Prompts, Tool Calling, and LangChain | LU-S2-09 to 12 | PR, CF, TP, HLA |
| Lesson 4 | RAG Architecture — Concepts and Design | LU-S2-13 to 16 | PR, CF, TP, HLA |
| Lesson 5 | Agentic AI — Concepts and Design Boundaries | LU-S2-17 to 20 | PR, CF, TP, HLA |
| Lab | Agent-Aware Prompt Templates & Structured Output Validation | LU-S2-21 to 22 | TP, TP |

**Approach Type Key:** PR = Problem Recognition | CF = Conceptual Framework | TP = Task Procedure | HLA = High-Leverage Application

---

## Session Learning Objectives

By the end of Session 2, participants will be able to:

1. Construct a layered prompt (system, developer, user) that enforces consistent application behavior across multiple callers and input types.
2. Select the appropriate prompting pattern (zero-shot, few-shot, chain-of-thought, or ReAct) for a given task based on complexity and output precision requirements.
3. Debug a failing prompt by reading API traces and logs rather than by inspecting output text alone.
4. Implement LangChain prompt templates with parameterised variables and Pydantic output parsers for maintainable, validated pipelines.
5. Design a RAG ingestion and retrieval pipeline for a defined document-grounded use case and measure retrieval precision empirically.
6. Correctly diagnose whether a task requires an agentic architecture or a structured pipeline with explicit conditional logic.
7. Design a minimal viable agent loop with a defined tool set, explicit stopping rule, and iteration limit.
8. Implement structured output validation with automatic retry on schema failure in any LLM pipeline.

---

## Lesson Summaries

### Lesson 1 — Prompt Structure and Layering (LU-S2-01 to 04)

Flat prompts — single instruction strings that mix behavioral guidance, task specification, and user input — produce inconsistent outputs across callers because the model has no stable hierarchy. The model reweights instructions based on how the user phrases their message, causing behavioral drift that is invisible in controlled testing but surfaces immediately in shared production use.

The fix is structural separation: the system prompt defines the model's persistent behavioral contract (role, output format, constraints, and handling of ambiguous input); a developer or operator layer passes dynamic per-call context; the user message is the only input the end user controls. A well-written system prompt is not a task description — it is a specification precise enough that behavioral problems trace to the prompt's wording, not to user input variation.

Key concepts: system prompt as behavioral contract, prompt layer hierarchy, adversarial input testing, version-controlled system prompts as application logic.

---

### Lesson 2 — Shot Patterns, Chain-of-Thought, and ReAct (LU-S2-05 to 08)

Zero-shot prompts applied to complex tasks produce variable performance because the model infers the full specification of correct output from a description rather than from examples. Failures cluster at edge cases and boundary conditions that the instruction describes but does not demonstrate.

Pattern selection is a design decision, not a stylistic preference. Zero-shot is the right starting point for common, well-defined tasks. Few-shot augments the instruction with representative demonstration pairs — real inputs covering the actual distribution, not idealized examples — and is most valuable for custom output formats and ambiguous classification decisions. Chain-of-thought prompts the model to reason through a problem step by step before producing a final answer, improving performance on multi-step inference and making conclusions reviewable. ReAct combines reasoning and acting — the foundation pattern for agentic applications.

The highest-value application in production is matching chain-of-thought to any task where the path to the conclusion matters for reviewer confidence, and few-shot to any task where schema deviation in downstream systems constitutes a system failure.

Key concepts: zero-shot, one-shot, few-shot, chain-of-thought elicitation by instruction vs. demonstration, ReAct, tail failure reduction vs. mean accuracy improvement.

---

### Lesson 3 — Debugging Prompts, Tool Calling, and LangChain (LU-S2-09 to 12)

Many prompt failures are only visible in traces, not in output text. Silent truncation near the context limit, unfilled template variables, instruction positions that receive reduced attention weight in long contexts, and schema mismatches that propagate to downstream systems — none of these are diagnosable by reading the model's output. Every production integration requires a trace that captures the exact assembled prompt string, the raw response, token counts for both, and a timestamp.

Function calling (tool use) converts tool invocation from a free-text interpretation problem into a structured, predictable interface. The model generates a structured invocation specifying the function name and arguments; the application layer executes the function and returns the result. LangChain's prompt templates separate stable instructional content from dynamic runtime variables, making prompts maintainable as application logic, independently testable, and version-traceable. Output parsers attached to every model call that generates structured data convert silent schema failures into catchable, actionable errors.

Key concepts: trace-level vs. output-level debugging, function calling as structured tool invocation, LangChain PromptTemplate, Pydantic output parsing, generate-validate-retry pattern.

---

### Lesson 4 — RAG Architecture: Concepts and Design (LU-S2-13 to 16)

Persistent hallucination on document-grounded tasks is not a prompting problem and not a model capability gap — it is an architectural limitation of transformer attention over long contexts. The mechanism by which a model should connect a query to the correct passage in a long document is not reliable enough for factual precision at high context lengths and information density. Retrieval-augmented generation addresses this by locating the relevant content first and passing only that content to the model as a short, targeted context.

A RAG system has two phases. Ingestion converts documents into a searchable vector index through chunking (300-500 tokens with overlap for dense prose, smaller for high-density documents), embedding, and storage. Retrieval embeds the query, computes similarity against stored chunk embeddings, and returns the top-k results. Hybrid retrieval — combining embedding similarity with keyword matching — outperforms either approach alone for most knowledge-retrieval tasks.

RAG is not the right architecture when documents change faster than ingestion can keep pace, when queries require full-corpus synthesis rather than passage retrieval, or when the grounding information is not in retrievable text form. It does not solve reasoning errors; if retrieved context is correct and the model still generates incorrect output, the problem is in the generation layer.

Key concepts: chunking strategy and overlap, embedding model selection, cosine similarity, dense vs. sparse vs. hybrid retrieval, retrieval precision evaluation, RAG vs. long-context direct generation decision boundary.

---

### Lesson 5 — Agentic AI: Concepts and Design Boundaries (LU-S2-17 to 20)

Agentic architectures are commonly applied to problems that are not agent problems. A pipeline with bounded, classifiable input variation — the kind that could be handled with better input classification and a few targeted prompt variants — does not need an agent. Adding autonomy to a problem that required specificity produces behavior that is less predictable and harder to debug than the original pipeline.

The agent loop runs in four phases: plan (decompose the task), act (invoke a tool), observe (process the tool result), and refine (assess completion and decide next action). Reasoning makes planning visible and reduces unnecessary tool invocations. Tool use is the action mechanism — the model generates structured invocations that the application layer executes. Autonomy is the degree of loop execution without human checkpoints, and high autonomy is appropriate only for low-risk, well-bounded tasks with reliable tool interfaces.

In professional services contexts, the operative principle is that agents may prepare and propose external actions but should not execute them without explicit human confirmation. Full automation is appropriate for internal pipeline steps where errors are visible, recoverable, and low-consequence. It is not appropriate for client-facing outputs or external actions at this stage of the technology's maturity.

Key concepts: agent loop (plan-act-observe-refine), tool set minimisation, explicit stopping rules, iteration limits, human-in-the-loop architecture, diagnostic question for agent vs. pipeline selection.

---

## Deliverables and Files

| File | Purpose |
|------|---------|
| `McK_GenAI-Tech-Intro_Session2_LUs.docx` | Authoritative source content — 22 Learning Units across 5 lessons and 1 lab |
| `Session2_HandsOn_Lab.ipynb` | Hands-on lab notebook — 30 cells, 15 code, 15 markdown, 903 lines |
| `Session2_README.md` | This file — instructor and participant orientation guide |

---

## Prerequisites

### Python Environment

```bash
python --version   # Requires Python 3.9 or higher
```

### Required Packages

```bash
pip install openai langchain langchain-openai pydantic faiss-cpu tiktoken numpy
```

### API Access

```bash
export OPENAI_API_KEY="sk-..."
```

The notebook uses `gpt-4o-mini` for generation and `text-embedding-3-small` for embeddings. Both are available on a standard OpenAI API key. To use a different provider (Azure OpenAI, Anthropic, etc.), update the client initialisation in the Setup cell.

### Verification

Run the Setup cell in the notebook. You should see:

```
Setup complete. API client ready.
```

---

## Lab Quick-Start (5 Minutes)

```python
# 1. Confirm API access
from openai import OpenAI
client = OpenAI()
resp = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": "Reply with one word: ready"}],
    max_tokens=5,
)
print(resp.choices[0].message.content)  # Expected: "ready"

# 2. Confirm embeddings
emb = client.embeddings.create(model="text-embedding-3-small", input=["test"])
print(f"Embedding dim: {len(emb.data[0].embedding)}")  # Expected: 1536

# 3. Confirm FAISS
import faiss, numpy as np
idx = faiss.IndexFlatIP(8)
idx.add(np.random.rand(3, 8).astype("float32"))
print(f"FAISS index ready. {idx.ntotal} vectors.")  # Expected: 3
```

---

## Notebook Structure

The notebook follows a consistent four-cell pattern for each lesson that mirrors the LU structure:

1. **Markdown cell** — Problem Recognition + Conceptual Framework (PR + CF LUs): the failure pattern the lesson addresses, the concepts that explain it, and a reference table or framework summary.
2. **Code cell(s)** — Task Procedure (TP LUs): working implementation of the procedure described in the LU, with inline comments that cross-reference LU IDs.
3. **Code cell** — High-Leverage Application (HLA LUs): a production-oriented extension or evaluation of the preceding implementation.
4. **Markdown cell** — Exercise: a bounded challenge that requires the participant to extend, adapt, or evaluate the preceding code independently.

The lab section at the end (LU-S2-21 and LU-S2-22) integrates all five lessons into a complete pipeline: agent-aware prompt template, agent execution loop with trace logging, Pydantic output schema, and a validation loop with automatic retry on schema failure.

### Cell Reference Map

| Cells | Content | LU IDs |
|-------|---------|--------|
| 0 | Title, session roadmap, prerequisites | — |
| 1-2 | Environment setup, `chat()` wrapper, trace wrapper | — |
| 3-7 | Lesson 1: Layered prompts, adversarial testing, Exercise 1 | LU-S2-01 to 04 |
| 8-11 | Lesson 2: Zero-shot vs. few-shot, chain-of-thought, Exercise 2 | LU-S2-05 to 08 |
| 12-16 | Lesson 3: Trace logging, function calling, LangChain templating, Exercise 3 | LU-S2-09 to 12 |
| 17-21 | Lesson 4: RAG pipeline, FAISS index, retrieval evaluation, Exercise 4 | LU-S2-13 to 16 |
| 22-24 | Lesson 5: Agent loop, tool execution, Exercise 5 | LU-S2-17 to 20 |
| 25-28 | Lab: Agent-aware template, Pydantic validation, retry loop, Lab extension | LU-S2-21 to 22 |
| 29 | Reflection: self-check table, 30-day commitment | — |

---

## Connection to the Rest of the Program

**Session 2 builds on Session 1** by taking model behavior knowledge (how transformers process tokens, why attention degrades at long contexts, what temperature controls) and converting it into engineering decisions. The prompt layering work in Lesson 1 applies directly to the parameter selection work in Session 1's Lab. The explanation of why RAG solves a structural attention problem (Lesson 4) is only accessible if participants understand transformer attention from Session 1.

**Session 3 (Track A — RAG Capstone)** extends Lesson 4 directly. Participants build a production-grade RAG pipeline in Track A's lab. The retrieval evaluation methodology introduced here (precision@k against a ground-truth eval set) is the evaluation framework they use in Track A.

**Session 3 (Track B — Agentic Capstone)** extends Lesson 5 directly. Participants build a multi-tool agent scaffold in Track B's lab. The agent loop design, tool set definition, and stopping rule specification from LU-S2-19 are the design artifacts they produce in Track B.

**Session 4 (Cross-Track Debrief)** requires participants to present their capstone work. The structured output validation framework from the lab (LU-S2-22) informs the discussion of review standards and output quality governance in Session 4's broader implications segment.

---

## Session 2 Outcome

A participant who completes Session 2 and its lab can:

1. Take a production LLM integration with inconsistent behavior across callers and fix it by restructuring the prompt hierarchy — without changing the model or the task.
2. Look at a failing prompt and correctly identify whether the fix is a shot pattern upgrade, a chain-of-thought addition, a trace-level diagnosis, or a RAG architecture change.
3. Build a LangChain-based pipeline with versioned templates, Pydantic output validation, and automatic retry logic — and commit it to a repository as application code, not as an embedded string.
4. Design a minimal viable agent for a defined task and identify correctly whether that task warrants an agent or a structured pipeline.

---


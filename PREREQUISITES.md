# ✅ Prerequisites — Read This Before Touching Any Code

Welcome! Before you clone the repo and start coding, take 10 minutes to work through this checklist. Every item here is something that, if missed, will surface as a confusing error mid-lab. Checking them now means your lab time is spent on GenAI concepts, not environment firefighting.

---

## 1. 🐍 Python Version

You need **Python 3.9 or later**.

```bash
python3 --version
# Expected output: Python 3.9.x or higher
```

If your version is older, install a newer interpreter. We recommend [pyenv](https://github.com/pyenv/pyenv) for managing multiple Python versions on macOS/Linux, or downloading directly from [python.org](https://www.python.org/downloads/) on Windows.

> **Why 3.9?** LangChain and Pydantic v2 both require Python 3.9+ for type-hinting features they use internally. Running on 3.8 will produce subtle import errors.

---

## 2. 🔑 API Keys

### OpenAI API Key *(Required for both tracks)*

Both the RAG pipeline (Track A) and the Agentic pipeline (Track B) make calls to OpenAI's API — for embeddings (`text-embedding-ada-002`) and for chat completions (`gpt-4o-mini` or your cohort's approved model).

**To get your key:**
1. Create or log in to an account at [platform.openai.com](https://platform.openai.com)
2. Navigate to **API Keys** → **Create new secret key**
3. Copy the key immediately — you won't be able to see it again

**Expected cost at lab scale:** Running through the full lab with a small corpus (20–50 documents) typically costs less than **$1.00 USD** in API credits. Set a monthly spending limit in your OpenAI account settings to avoid surprises.

> **Corporate environments:** If you are working inside a managed environment (e.g., an enterprise Azure OpenAI deployment), your facilitator will provide the endpoint URL and key. You may need to set `AZURE_OPENAI_ENDPOINT` and `OPENAI_API_TYPE=azure` instead of the standard key — check with your facilitator.

---

## 3. 💻 Required Software

### Python Virtual Environment tooling

The standard `venv` module ships with Python 3.3+. No separate installation needed. Always create a fresh environment for this project to isolate its dependencies:

```bash
python3 -m venv genai_env
source genai_env/bin/activate    # macOS / Linux
# genai_env\Scripts\activate     # Windows PowerShell
```

### Git

You'll need Git to clone the repository:

```bash
git --version
# Expected: git version 2.x.x
```

Download from [git-scm.com](https://git-scm.com) if not installed.

### Jupyter (for Sessions 1 & 2 notebooks)

Jupyter is included in `requirements.txt` and will be installed when you run `pip install -r requirements.txt`. No separate installation needed. Launch it with:

```bash
jupyter notebook
```

### Docker *(Optional — for Module 7 only)*

Module 7 (Deploying a GenAI Microservice) requires Docker. This module is not part of the one-day core curriculum, but if you want to explore it:

- Install Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop/)
- Verify with: `docker --version`

---

## 4. 📋 Assumed Background Knowledge

This course is designed for technically capable participants. You'll get the most out of it if you arrive with:

| Skill | Level Required |
|---|---|
| Python programming | Comfortable writing and reading Python scripts |
| Running Jupyter notebooks | Basic — opening, running cells, reading output |
| JSON | Familiar with reading and writing JSON structures |
| REST APIs | Helpful but not required — we explain what we use |
| Machine Learning / AI fundamentals | Not required — Session 1 builds this from scratch |

If Python is new to you, spending 2–3 hours on a free beginner tutorial (e.g., [learnpython.org](https://www.learnpython.org)) before the session will meaningfully improve your experience.

---

## 5. 🔐 Pro-Tip: Managing Your `.env` File Securely

Your `.env` file holds secrets — API keys that can rack up charges if exposed. Treat it like a password.

### The right setup

```bash
# Step 1: Copy the template
cp .env.example .env

# Step 2: Add your key (open .env in any text editor)
OPENAI_API_KEY=sk-proj-xxxxxxxxxxxxxxxxxxxxxxxx

# Step 3: Verify it's in .gitignore (it already is in this repo)
cat .gitignore | grep .env
# Should output: .env
```

### The golden rules

**✅ DO:**
- Store all secrets in `.env` and load them at runtime via `python-dotenv`
- Confirm the key is loading by printing only the **first 4 characters**: `print(os.getenv('OPENAI_API_KEY')[:4])`
- Add `.env` to your `.gitignore` **before** your first `git commit` (this repo already does this)
- Rotate your key immediately if you think it was ever committed to version control

**❌ NEVER:**
- Hardcode your API key directly in a `.py` file or notebook
- Commit your `.env` file to GitHub — OpenAI's bots scan public repos and auto-revoke any exposed keys within minutes
- Share your `.env` file or paste its contents in Slack, email, or a chat window
- Print the full key value to the console or into a log file

```python
# ✅ CORRECT — load from .env, never hardcode
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.getenv("OPENAI_API_KEY")

# Quick sanity check — print ONLY the first 4 characters
print(f"Key loaded: {api_key[:4]}...")
```

```python
# ❌ WRONG — never do this
openai.api_key = "sk-proj-abc123..."   # hardcoded → instantly leaks if pushed to GitHub
```

> 💡 **Extra layer of safety:** For personal projects beyond this lab, consider using a secrets manager like [1Password Secrets Automation](https://1password.com/secrets/) or environment variables set at the OS level, so secrets never live in a file at all.

---

## 6. 🔍 Verification Checklist

Run through this before the lab session begins:

```bash
# Python version ≥ 3.9
python3 --version

# Virtual environment activated (you should see "(genai_env)" in your prompt)
which python3

# All packages installed
pip list | grep -E "openai|langchain|faiss|tiktoken|python-dotenv"

# API key loads correctly
python3 -c "from dotenv import load_dotenv; import os; load_dotenv(); print(os.getenv('OPENAI_API_KEY')[:4])"

# Minimal OpenAI API call succeeds
python3 scripts/verify_env.py
```

If any step fails, check the error message against the troubleshooting notes in each lab's `README.md`, or ask your facilitator before the session starts.

---

## 7. 📞 Getting Help

| Channel | Use for |
|---|---|
| Your facilitator | Environment issues before/during the session |
| Lab `README.md` files | Step-by-step troubleshooting for each track |
| [OpenAI Status Page](https://status.openai.com) | Checking for API outages |
| [LangChain Docs](https://python.langchain.com) | LangChain API reference and migration guides |

You're set. Now go build something real. 🚀

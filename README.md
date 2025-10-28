# HUMAN-REWRITE-AI
(AI-powered text humanizer that rewrites robotic or AI-generated text into natural human language using open-source NLP models.)
A lightweight, cloud-ready project that rewrites AI-generated or robotic text into natural, human-like language while keeping the original meaning. Built with FastAPI, Gradio, and transformer models such as T5, Pegasus, and Sentence-BERT, this project demonstrates end-to-end AI engineering from model integration to cloud deployment using free tiers.
HumanRewrite AI is a lightweight, cloud-ready system that rewrites AI-generated or robotic-sounding text into fluent, natural human language while preserving meaning.
It combines modern NLP techniques with strong use of FastAPI, Gradio, and open-source transformer models like T5, Pegasus, and Sentence-BERT.
The project demonstrates full end-to-end AI engineering: from text processing and model integration to API deployment and cost optimization on free cloud tiers.

### HumanRewrite MVP — AI Text Humanizer (Zero-Cost, Cloud-Ready)

A production-grade, free-tier MVP that rewrites AI-like text into fluent, human-sounding prose while preserving meaning. The system prioritizes robust engineering (tests, CI/CD, caching, batching) with lightweight generative AI (T5/Pegasus + similarity/perplexity/readability scoring).

#### Key Features

* FastAPI JSON API (/rewrite, /metrics, /health)
* Gradio demo UI
* Candidate generation using small open models (no paid APIs)
* Scoring layer: semantic similarity (Sentence-BERT), perplexity (GPT-2 small), readability (textstat), repetition/burstiness checks
* Cost optimization via caching, batching, rate-limiting, and early exits
* One-click deploy to Hugging Face Spaces (Docker) + GitHub Actions CI

#### High-Level Architecture

```
Client (Gradio UI) -> FastAPI (/rewrite)
                      -> Model Engine (T5/Pegasus)
                      -> Scoring (SBERT, GPT-2, textstat)
                      -> Cache (in-memory + disk)
```

#### Endpoints

* GET /health – service status
* POST /rewrite – {text, options?} -> {best, candidates[], scores}
* POST /metrics – {text} -> {similarity, perplexity, readability, repetition, entropy}

#### Tech Stack

Python 3.11, FastAPI, Uvicorn, Gradio, Transformers, Sentence-Transformers, textstat, NumPy, Pandas, diskcache, pytest, ruff/black, Docker, GitHub Actions, Hugging Face Spaces

#### Repository Layout

```
api/        # FastAPI app, models, scoring, caching, config
ui/         # Gradio demo
tests/      # Unit tests
.github/    # CI workflows
Dockerfile  # Container for Spaces
```

#### Quick Start

```
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
uvicorn api.main:app --reload
# In another terminal:
python ui/app.py
```

#### Roadmap

* Add multilingual support
* Quantized models for faster inference
* Advanced style controls (tone, formality)
* Optional paid LLM booster (toggleable)

---

## Step 2 — Bootstrap the Runnable Skeleton (Environment, Dependencies, Minimal Code)

**Objective:** Make the repository runnable end-to-end with a health check and placeholder /rewrite endpoint.

**Location:** Local environment (or GitHub Codespaces)

**Execution Order:** Step 2 (immediately after repository creation)

### Instructions

1. **Create a virtual environment and install dependencies**

```
python -m venv .venv
# macOS/Linux
source .venv/bin/activate
# Windows
# .venv\Scripts\activate

pip install --upgrade pip
pip install -r requirements.txt
```

2. **Add a minimal FastAPI app** (`api/main.py`)

```
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class RewriteReq(BaseModel):
    text: str

@app.get("/health")
def health():
    return {"ok": True}

@app.post("/rewrite")
def rewrite(req: RewriteReq):
    # placeholder: echo until models are added in Step 3
    return {"best": req.text, "candidates": [req.text], "scores": {"note": "stub"}}
```

3. **Add configuration and caching stubs**

`api/config.py`

```
import os

API_RATE_LIMIT = int(os.getenv("API_RATE_LIMIT", 30))  # requests per minute
MAX_INPUT_CHARS = int(os.getenv("MAX_INPUT_CHARS", 2500))
```

`api/caching.py`

```
from functools import lru_cache

@lru_cache(maxsize=2048)
def memo(key: str, value=None):
    # Call memo(key) to read; memo(key, value) to set
    if value is not None:
        memo.cache_clear()
    return value
```

4. **Wire up a simple Gradio UI** (`ui/app.py`)

```
import gradio as gr
import requests, os

API = os.getenv("API_URL", "http://127.0.0.1:8000")

def run(text):
    r = requests.post(f"{API}/rewrite", json={"text": text}, timeout=20)
    return r.json().get("best", "")

demo = gr.Interface(fn=run, inputs="textbox", outputs="textbox", title="HumanRewrite MVP", allow_flagging="never")

if __name__ == "__main__":
    demo.launch()
```

5. **Run locally**

```
uvicorn api.main:app --reload
# In another terminal:
python ui/app.py
```

6. **Add a basic test** (`tests/test_api.py`)

```
from fastapi.testclient import TestClient
from api.main import app

def test_health():
    c = TestClient(app)
    assert c.get("/health").json()["ok"] is True
```

Run tests:

```
pip install pytest
pytest -q
```

7. **Commit and push changes**

```
git add .
git commit -m "Step 2: runnable skeleton with health + stub rewrite"
git push
```

**Purpose:** Establish a working, tested, container-ready base before adding AI models. This provides fast feedback loops and aligns with the project's emphasis on strong engineering foundations.

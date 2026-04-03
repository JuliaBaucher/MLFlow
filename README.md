# MLflow Integration — LLM Evaluation & Experiment Tracking

## Overview

This project integrates MLflow into a local AI/LLM evaluation pipeline to track, analyze, and compare prompt performance for a chatbot system.

The setup uses:
- A local MLflow tracking server (SQLite backend)
- A Python evaluation script (`test_chatbot_mlflow.py`)
- A REST-based LLM inference endpoint
- Manual and automated evaluation metrics

---

## What Has Been Implemented

### 1. MLflow Tracking Server (Local)

- Deployed a local MLflow server:
  - Backend store: SQLite (`mlflow.db`)
  - Artifact storage: local filesystem (`./mlruns`)
  - UI available at: `http://127.0.0.1:5000`

- Configured tracking via:
  - `MLFLOW_TRACKING_URI=http://127.0.0.1:5000`

---

### 2. LLM Evaluation Pipeline

- Developed a Python script (`test_chatbot_mlflow.py`) that:
  - Sends prompts to an LLM API
  - Measures response latency
  - Captures responses
  - Evaluates output quality (keyword match + manual scoring)

---

### 3. Experiment Tracking

For each test run, the following is logged to MLflow:

#### Inputs
- Prompt version (e.g. `v1`, `v2`)
- User query

#### Outputs
- Model response (text)
- Structured JSON response (artifact)

#### Metrics
- Latency (ms)
- Keyword match rate
- Manual evaluation score (1–5)

---

### 4. Prompt Versioning & Comparison

- Multiple prompt versions tested (e.g. `projects_list_v1`, `projects_list_v2`)
- Enables:
  - Side-by-side comparison in MLflow UI
  - Iterative prompt improvement
  - Performance benchmarking

---

### 5. Artifact Logging

- Full responses stored as artifacts:
  - `response.json`
- Enables:
  - Traceability
  - Debugging
  - Offline analysis

---

## Key Capabilities

- End-to-end evaluation loop:
  - Prompt → LLM → Evaluation → Logging
- Structured experiment tracking for LLM systems
- Combination of:
  - Quantitative metrics (latency, keyword match)
  - Qualitative scoring (manual rating)
- Local, lightweight setup (no cloud dependency)

---

## Architecture

Python Script (test_chatbot_mlflow.py)
↓
LLM API (HTTP call)
↓
Response + Metrics
↓
MLflow Tracking (HTTP)
↓
SQLite (metadata) + mlruns/ (artifacts)
↓
MLflow UI (visualization)



---

## Outcome

This setup provides a **simple but powerful framework** to:

- Evaluate LLM responses systematically
- Compare prompt versions
- Track performance over time
- Debug and audit model behavior

It serves as a **lightweight experimentation platform for GenAI / LLM product development**.



##Activation

# ================================
# MLflow — Restart (Your Setup)
# ================================

# 1. Activate your virtual environment
source ~/mlflow-test/bin/activate

# 2. (Optional but recommended) Set tracking URI
export MLFLOW_TRACKING_URI=http://127.0.0.1:5000

# 3. Start MLflow server
mlflow server \
  --backend-store-uri sqlite:///home/juliabaucher/mlflow.db \
  --default-artifact-root ./mlruns \
  --host 127.0.0.1 \
  --port 5000

# ================================
# Access MLflow UI
# ================================
# Open in browser:
# http://127.0.0.1:5000

# ================================
# Run your script (in another terminal)
# ================================

source ~/mlflow-test/bin/activate
export MLFLOW_TRACKING_URI=http://127.0.0.1:5000
python test_chatbot_mlflow.py

# ================================
# Troubleshooting
# ================================

# Check if MLflow is running
lsof -i :5000

# Kill process if port is busy
kill -9 $(lsof -t -i:5000)

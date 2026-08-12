# 🪙 Tharwa (ثروة) - Financial Advisory & Compliance Agent System

> **Student Name:** [Your Full Name]  
> **SDAIA Programme:** Advanced Agentic AI Systems Engineering  
> **Declared Track:** Track A: Finance & Compliance  
> **Date:** August 2026  

Tharwa is an intelligent, multi-agent financial advisory platform built with **LangGraph**, **LangChain**, and **Google Gemini**[span_0](start_span)[span_0](end_span). It features an **Evaluator-Optimizer compliance engine** grounded in Saudi Capital Market Authority (CMA) regulations, combined with real-time market data analysis, stateful user memory, and Human-in-the-Loop (HITL) authorization guardrails[span_1](start_span)[span_1](end_span).

---

## 📐 Architecture Overview

The system utilizes a **Multi-Agent Supervisor Pattern** to route queries dynamically and manage execution state across specialized worker agents[span_2](start_span)[span_2](end_span):

```
                       ┌─────────────────────────┐
                       │       START Node        │
                       └────────────┬────────────┘
                                    │
                                    ▼
                       ┌─────────────────────────┐
                       │    Supervisor Node      │
                       └────────────┬────────────┘
                                    │
           ┌────────────────────────┼────────────────────────┐
           │ (Router Edge)          │ (Router Edge)          │ (Router Edge)
           ▼                        ▼                        ▼
┌─────────────────────┐  ┌─────────────────────┐  ┌─────────────────────┐
│ Market Analysis Node│  │   Portfolio Node    │  │   Compliance Node   │
└──────────┬──────────┘  └──────────┬──────────┘  └──────────┬──────────┘
           │                        │                        │
           │ (Tools)                │ (Tools)                │ (Tools & RAG)
           │ • get_stock_price      │ • propose_allocation   │ • search_regulations
           │ • get_fundamentals     │                        │ • evaluate_compliance
           │ • tavily_search        │                        │ • optimize_strategy
           │                        │                        │
           └────────────────────────┼────────────────────────┘
                                    │
                                    ▼
                       ┌─────────────────────────┐
                       │ Human Approval Interrupt│ (HITL Guardrail)
                       └────────────┬────────────┘
                                    │
                                    ▼
                       ┌─────────────────────────┐
                       │        END Node         │
                       └─────────────────────────┘
```

---

## 🛠️ Key Components & Modules

### 1. **Supervisor Agent**
* Routes user queries based on intent (`market_analysis`, `portfolio_planning`, `compliance_check`)[span_3](start_span)[span_3](end_span).
* Coordinates intermediate agent execution outputs into a cohesive final response[span_4](start_span)[span_4](end_span).

### 2. **Market Analysis Agent**
* `get_stock_price`: Fetches price history via `yfinance`[span_5](start_span)[span_5](end_span).
* `get_company_fundamentals`: Retrieves key company financial metrics and valuations[span_6](start_span)[span_6](end_span).
* `tavily_search`: Searches for real-time news and market updates[span_7](start_span)[span_7](end_span).

### 3. **Portfolio Agent**
* `propose_allocation`: Constructs tailored asset allocation models based on risk tolerance, financial goals, and target horizons[span_8](start_span)[span_8](end_span).

### 4. **Compliance Agent (Evaluator-Optimizer RAG Engine)**
* `search_regulations`: Executes dense semantic vector search over chunked Saudi CMA regulatory documents using `paraphrase-multilingual-mpnet-base-v2` and `Chroma`[span_9](start_span)[span_9](end_span).
* `evaluate_compliance`: Evaluates proposed portfolios for regulatory breaches (e.g., maximum 60% asset class concentration cap)[span_10](start_span)[span_10](end_span).
* `optimize_strategy`: Iteratively adjusts portfolios to reach 100% compliance before output generation[span_11](start_span)[span_11](end_span).

### 5. **Human-in-the-Loop (HITL) Guardrail**
* Employs LangGraph's `interrupt()` function to pause execution before finalizing high-value investments or allocation changes until human authorization is received via `Command(resume=...)`[span_12](start_span)[span_12](end_span).

### 6. **Memory & State Persistence Layer**
* **Short-Term Checkpointer**: `InMemorySaver` tracks conversational context within thread turns[span_13](start_span)[span_13](end_span).
* **Long-Term Memory**: `InMemoryStore` records long-term user risk profiles across different sessions[span_14](start_span)[span_14](end_span).

---

## ⚙️ Configuration & Setup

### **Prerequisites**

* Python 3.10+
* Virtual Environment (`venv` or `conda`)

### **1. Installation**

Clone the repository and install the dependencies:

```bash
git clone [https://github.com/your-username/tharwa-financial-agent.git](https://github.com/your-username/tharwa-financial-agent.git)
cd tharwa-financial-agent
pip install -r requirements.txt
```

### **2. Environment Variables (`.env`)**

Create a `.env` file in the root directory:

```env
# Primary LLM API Key (Google Gemini)
GEMINI_API_KEY="your-google-gemini-api-key"

# Real-Time Web Search API Key
TAVILY_API_KEY="your-tavily-api-key"

# LangSmith Tracing
LANGCHAIN_TRACING_V2="true"
LANGCHAIN_API_KEY="your-langchain-api-key"
LANGCHAIN_PROJECT="tharwa-financial-agent"
```

### **3. Model & Hyperparameter Settings (`config.py`)**

```python
# LLM & Embedding Settings
LLM_MODEL = "gemini-2.0-flash"
EMBEDDING_MODEL = "sentence-transformers/paraphrase-multilingual-mpnet-base-v2"

# RAG & Chunking Parameters
CHUNK_SIZE = 1000
CHUNK_OVERLAP = 150
TOP_K_RETRIEVAL = 4

# Compliance Guardrails
MAX_SINGLE_ASSET_CONCENTRATION = 0.60  # Maximum 60% allowed in a single asset class
MAX_OPTIMIZATION_ROUNDS = 3           # Limit for Evaluator-Optimizer feedback loop
```

---

## 🚀 Usage Example (Standard & HITL Resume)

```python
from langgraph.types import Command
from tharwa.graph import app

# 1. Start execution with thread configuration
config = {"configurable": {"thread_id": "user_session_001"}}
input_message = {
    "messages": [
        {"role": "user", "content": "Propose an aggressive growth portfolio and verify it against CMA rules."}
    ]
}

# Stream graph until interrupt
for event in app.stream(input_message, config=config):
    print("State execution event:", event)

# 2. Resume execution after Human Review
resume_command = Command(resume={"approved": True, "notes": "Approved by supervisor."})
final_result = app.invoke(resume_command, config=config)

print("Final Output:", final_result)
```

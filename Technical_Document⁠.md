# Technical Documentation
## Tharwa (ثروة) – Advanced Agentic AI Financial Advisory & Compliance System

---

# 1. System Overview

**Tharwa (ثروة)** is an agentic AI financial advisory platform designed to deliver personalized investment strategies while enforcing automated regulatory compliance. Built for the Saudi financial ecosystem, it grounds asset allocations in Saudi Capital Market Authority (CMA) guidelines.

The system uses a LangGraph-based multi-agent supervisor architecture composed of stateful nodes representing specialized domain agents. It integrates Large Language Models (LLMs), dense retrieval-augmented generation (RAG), evaluator-optimizer control loops, and Human-in-the-Loop (HITL) authorization to generate compliant portfolio plans and real-time market insights.

---

# 2. Architecture

The system follows a multi-agent supervisor pattern implemented with LangGraph. Queries are routed dynamically by a central supervisor, and compliance evaluations are iteratively optimized before reaching human approval.

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
           │ (Tools)                │ (Tools)                │ (Evaluator-Optimizer RAG)
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
                        ┌───────────┴───────────┐
                        │                       │
                    Approved                Rejected
                        │                       │
                        ▼                       ▼
           ┌────────────────────────┐┌────────────────────────┐
           │ Finalize & Save State  ││    Cancel Request      │
           └───────────┬────────────┘└───────────┬────────────┘
                       │                         │
                       └────────────┬────────────┘
                                    │
                                    ▼
                       ┌─────────────────────────┐
                       │        END Node         │
                       └─────────────────────────┘
```

---

# 3. Graph Nodes & Sub-Agents

## Node 1 — Supervisor Agent

**Purpose**

Analyzes incoming user intent, coordinates execution between specialized agents, and aggregates intermediate responses into unified system outputs.

**Input**

- `messages` (Conversation state / User Query)

**Output**

- Dynamic routing handoff or final response synthesis

---

## Node 2 — Market Analysis Agent

**Purpose**

Retrieves financial metrics, historical price performance, and news updates for assets.

**Tools & Integrations**

- `get_stock_price`: Fetches price history via `yfinance`.
- `get_company_fundamentals`: Retrieves company valuation metrics and financial statements.
- `tavily_search`: Performs real-time web searches for market news.

---

## Node 3 — Portfolio Agent

**Purpose**

Constructs customized asset allocation strategies based on investor demographics, risk profiles, and investment horizons.

**Tools**

- `propose_allocation`: Generates structured portfolio weightings across equities, fixed income, cash, and alternative asset classes.

---

## Node 4 — Compliance Agent (Evaluator-Optimizer RAG Engine)

**Purpose**

Grounds proposed portfolios against Saudi Capital Market Authority (CMA) legal regulations using dense semantic retrieval and iterative optimization loops.

**Technologies & Tools**

- **RAG Retriever (`search_regulations`)**: Performs dense vector search over chunked CMA regulations using `paraphrase-multilingual-mpnet-base-v2` and ChromaDB.
- **Evaluator (`evaluate_compliance`)**: Scans portfolio weightings against compliance constraints (e.g., maximum 60% concentration cap in a single asset class).
- **Optimizer (`optimize_strategy`)**: Adjusts portfolio allocations to resolve detected regulatory breaches.

---

## Node 5 — Human-in-the-Loop (HITL) Interrupt Node

**Purpose**

Pauses graph execution before high-risk operations (e.g., portfolio rebalancing or trade execution) to demand explicitly structured human review.

**Implemented Using**

- LangGraph `interrupt()`
- `Command(resume=...)`

---

## Node 6 — State Finalization / Termination

**Purpose**

Saves finalized portfolio states to persistent storage if approved, or halts workflow safely if rejected.

---

# 4. State & Memory Management

The application manages dual-layer state persistence using LangGraph Checkpointers and Stores:

## Short-Term Memory (Checkpointer)
- **Component**: `InMemorySaver` (or SQLite checkpointer)
- **Role**: Tracks thread-level conversational state, message history, and execution context per active `thread_id`.

## Long-Term Memory (Store)
- **Component**: `InMemoryStore` (or persistent key-value store)
- **Role**: Maintains persistent user profiles, historical risk tolerance ratings, and preferences across independent session threads.

### State Variables
- `messages`: List of conversation messages
- `risk_profile`: User risk profile metadata
- `portfolio_draft`: Current proposed asset allocations
- `compliance_status`: Outcome of CMA evaluation loops
- `approval_status`: Outcome of HITL review

---

# 5. Human-in-the-Loop (HITL) Workflow

High-risk actions pause automatically using LangGraph's native `interrupt()` function.

```
                     Execution Pauses at Node
                                │
                                ▼
                   Human Operator Review Prompt
                                │
                  ┌─────────────┴─────────────┐
                  ▼                           ▼
            Approve Action              Reject Action
                  │                           │
                  ▼                           ▼
     Command(resume={"approved": True})   Command(resume={"approved": False})
                  │                           │
                  ▼                           ▼
          Resume Graph Runs           Cancel Execution
```

---

# 6. Technologies Used

| Component | Technology |
|------------|------------|
| Workflow Engine | LangGraph |
| Multi-Agent Router | `langgraph_supervisor` |
| LLM Framework | LangChain / LangChain Core |
| Primary LLM | Google Gemini (`gemini-2.0-flash`) |
| Vector Database | ChromaDB |
| Embedding Model | `sentence-transformers/paraphrase-multilingual-mpnet-base-v2` |
| Market Data APIs | `yfinance`, Tavily Search API |
| Observability | LangSmith (`LANGCHAIN_TRACING_V2`) |
| Programming Language | Python 3.10+ |
| Persistence Layer | LangGraph Saver & Store |

---

# 7. Project Structure

```
tharwa-financial-agent/
│── tharwa/
│   ├── __init__.py
│   ├── graph.py               # Main LangGraph multi-agent graph definition
│   ├── agents/                # Supervisor, Portfolio, Market, & Compliance agent implementations
│   ├── tools/                 # Financial metrics, Tavily search, and RAG retrieval tools
│   ├── config.py              # System hyperparameter and model configs
│   └── state.py               # Pydantic schemas and state definitions
│── data/
│   └── cma_regulations/       # Saudi Capital Market Authority PDF/text legal docs
│── vectorstore/               # Persisted ChromaDB vector embeddings
│── project.ipynb              # Execution and evaluation Jupyter Notebook
│── README.md                  # Project overview and metadata
│── Technical_Document.md      # Full architecture technical specification
│── Dockerfile                 # Containerization instructions
│── docker-compose.yml         # Container orchestration configuration
│── requirements.txt           # Python dependency locks
```

---

# 8. Deployment

The application includes production deployment configurations for containerized setups.

**Deployment Artifacts:**
- `Dockerfile`: Multi-stage Python build setup
- `docker-compose.yml`: Services setup for graph engine, local Chroma vector store, and API endpoints
- `requirements.txt`: Environment package locks

---

# 9. Configuration

Core system parameters defined in `config.py`:

```python
# LLM & Embedding Settings
LLM_MODEL = "gemini-2.0-flash"
EMBEDDING_MODEL = "sentence-transformers/paraphrase-multilingual-mpnet-base-v2"

# RAG Parameters
CHUNK_SIZE = 1000
CHUNK_OVERLAP = 150
TOP_K_RETRIEVAL = 4

# Compliance Guardrails
MAX_SINGLE_ASSET_CONCENTRATION = 0.60  # Maximum 60% allowed in a single asset class
MAX_OPTIMIZATION_ROUNDS = 3           # Limit for Evaluator-Optimizer feedback loop
```

---

# 10. Execution Flow

1. **User Request**: User submits a financial request (e.g., *"Propose a growth portfolio and check CMA compliance"*).
2. **Supervisor Routing**: The Supervisor Agent evaluates intent and hands off execution to the Portfolio or Market Analysis Agent.
3. **Draft Generation**: Portfolio Agent generates initial asset weightings based on user risk parameters.
4. **Evaluator-Optimizer RAG**: Compliance Agent retrieves relevant CMA rules via ChromaDB, checks for concentration breaches, and iteratively corrects portfolio weightings if needed.
5. **HITL Interruption**: Graph execution hits `interrupt()`, awaiting human compliance officer review.
6. **Command Resume**: Human operator sends a `Command(resume=...)` payload.
7. **Finalization / Termination**: Approved plans are logged to long-term memory and returned; rejected plans halt safely.

---

# 11. Production Readiness & Guardrails

- **Observability**: Complete end-to-end trace logging configured via LangSmith (`LANGCHAIN_TRACING_V2`).
- **Transient Error Resilience**: Configured with `RetryPolicy(max_attempts=3, backoff_factor=2.0)` on remote API tasks.
- **Regulatory Guardrails**: Strict concentration caps preventing algorithmically generated compliance violations.
- **State Isolation**: Short-term conversational checkpoints isolated per `thread_id` with cross-thread persistent user stores.

---

# 12. Future Improvements

- Direct integration with Tadawul (Saudi Exchange) live streaming ticker APIs.
- Production PostgreSQL + `pgvector` store for scalable multi-tenant RAG and checkpointer storage.
- Automated generation of Arabic regulatory audit reports in PDF format.
- Voice-activated input/output interface support using Gemini multi-modal capabilities.

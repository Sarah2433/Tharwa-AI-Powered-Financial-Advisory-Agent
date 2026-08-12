
# Tharwa

> **Track / Program:** [Insert Training Program Name Here]  
> **Cohort:** [Insert Cohort Name/Number Here]  
> **Trainee Name:** Sara  
> **Institution:** King Saud University (KSU) - College of Computer and Information Sciences  

---

## 🌟 Project Description
**Tharwa** is an agentic AI system specifically designed to assist market investors in making informed, data-driven investment decisions. The system analyzes market indicators, evaluates available assets, and matches them with the user's personal profile and risk level to deliver customized asset allocation recommendations while maintaining strict adherence to local regulatory frameworks.

---

## 🏗️ System Architecture

Tharwa follows a **Multi-Agent Orchestration** pattern, where specialized agents collaborate to provide tailored financial insights.

```text
+-------------------------------------------------------------------------+
|                              USER INTERFACE                             |
|                           [ Streamlit UI App ]                          |
+-------------------------------------------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+
|                      INTELLIGENT AGENT ORCHESTRATOR                     |
|                                                                         |
|  [1. Data Ingestion]   [2. Risk Analysis]   [3. Regulatory Guard]       |
|          |                     |                      |                 |
|          v                     v                      v                 |
|  [(Market Data API)]    [(User Profile DB)]   [(CMA Regulations)]       |
+-------------------------------------------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+
|                       FINAL OUTPUT & STRATEGY                           |
|                    [ Custom Asset Allocation ]                          |
+-------------------------------------------------------------------------+


```
---
##Architecture Components
-**Orchestrator Agent / Supervisor:** Routes user requests dynamically between specialized agents (Market Analysis, Portfolio Allocation, and Regulatory Compliance).

-  **Data Ingestion & Market Engine:** Connects to real-time financial tools (⁠yfinance⁠ and ⁠TavilySearch⁠) to fetch live stock prices, P/E ratios, market caps, and financial news.
 Evaluator-Optimizer Compliance Loop: Acts as an automated oversight mechanism, checking proposed portfolio allocations against Saudi CMA regulations and iterating up to 3 rounds to resolve violations.

- **Agentic RAG & Vector Store:** Employs ChromaDB with multilingual sentence-transformers (⁠paraphrase-multilingual-mpnet-base-v2⁠) to retrieve exact legal text from Saudi capital market rules and investment fund regulations.

- **State Management & Persistence:** Utilizes LangGraph checkpoints (In-Memory and SQLite-backed) alongside long-term semantic memory for cross-thread user profiling.

___


## 🎯 Key Features
- **Live Market Diagnostics:** Automatically extracts stock trends, fundamental metrics, and recent market sentiment.
-**Personalized Asset Allocation:** Customizes portfolio structures (Equities, Sukuk, Cash) based on investor risk levels (Conservative, Moderate, Aggressive) and investment horizons.

- **Automated CMA Regulatory Compliance:** Evaluates strategies against official Saudi Capital Market Authority guidelines and investment fund regulations.

- **Human-in-the-Loop (HITL) Workflow:** Incorporates interrupt-driven approval steps, allowing human review and modification of final portfolio recommendations before execution.

- **Robust Error Handling & Retries:** Implements automatic recovery policies for connection failures and transient API errors.
___





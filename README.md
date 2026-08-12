# Tharwa — AI-Powered Financial Advisory Agent

Tharwa is a multi-agent system built with LangChain and LangGraph that analyzes market data, evaluates investment options, and proposes a personalized asset allocation based on the user's risk tolerance and investment goal.

## Overview

The user provides an amount, a risk level, and an investment goal. A supervisor agent routes the request through specialized sub-agents — Market Analysis, Financial Analysis, and Risk Analysis — running in parallel. Their outputs feed into a Portfolio Allocation agent, which checks its recommendation against Saudi investment regulations via a Regulatory Compliance agent before requesting explicit human approval and returning a final recommendation.

## Architecture
User → Supervisor Agent
├── Market Analysis Agent
├── Financial Analysis Agent
└── Risk Analysis Agent
↓
Portfolio Allocation Agent
↓
Regulatory Compliance Agent
↓
Human Approval
↓
Final Recommendation

## Key Features

- **Real tool calls**: live market data retrieval with structured (Pydantic) output
- **LLM-based routing**: a supervisor agent classifies and dispatches requests to specialized sub-agents
- **Retrieval-Augmented Generation (RAG)**: grounded in Saudi Capital Market Authority (CMA) regulations and investment fund frameworks
- **Short-term & long-term memory**: per-conversation state via a checkpointer, and durable user preferences via a cross-thread Store
- **Human-in-the-loop**: an explicit approval step pauses execution before any final recommendation is issued
- **Built on LangGraph's Functional API**: `@task` / `@entrypoint`, with retry and fallback error-handling strategies
- **Workflow pattern**: Orchestrator-Worker, coordinating parallel analysis agents into a single recommendation
- **Observability**: full execution tracing via LangSmith

## Tech Stack

- LangChain / LangGraph
- Groq (LLM inference)
- HuggingFace Embeddings
- LangSmith (tracing)
- Python, Google Colab

## Disclaimer

Tharwa is an educational project built for a capstone assignment. It is not a licensed financial advisory service, and its output should not be treated as professional investment advice.

## Team
- Sarah Al Sa'ed
- Noura Alfaadhel
- Joud Aldawsari
- Norah Al Juwair
- Reema

## Program Attribution 
- **Course :** Buliding Agentic AI System 
- **Trainer :** Mohammed Albelad
- **Presented by :** SDAIA Academy 

SDAIA Academy GitHub :

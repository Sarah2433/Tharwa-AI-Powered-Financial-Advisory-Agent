# Tharwa — AI-Powered Financial Advisory Agent

The Tharwa project is a cutting-edge breakthrough in FinTech. The core idea is to build a Multi-Agent AI System that goes beyond generic advice, acting as an independent digital financial advisor that makes data-driven investment decisions tailored to each user's risk appetite and investment goals.

## Overview

The user provides an amount, a risk level, and an investment goal. A supervisor agent routes the request through specialized sub-agents — Market Analysis, Financial Analysis, and Risk Analysis — running in parallel. Their outputs feed into a Portfolio Allocation agent, which checks its recommendation against Saudi investment regulations via a Regulatory Compliance agent before requesting explicit human approval and returning a final recommendation.

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

```mermaid
graph TD
    User[User Input] --> Interface[Streamlit UI]
    Interface --> Orchestrator[Orchestrator Agent]
    
    subgraph Agents["Intelligent Agent Layer"]
        Orchestrator --> DataAgent[Data Ingestion Agent]
        Orchestrator --> RiskAgent[Risk Analysis Agent]
        Orchestrator --> ComplianceAgent[Regulatory Guard Agent]
    end
    
    DataAgent --> MarketData[(Market Data API)]
    RiskAgent --> UserProfile[(User Profile DB)]
    ComplianceAgent --> RegDB[(CMA Regulations DB)]
    
    DataAgent & RiskAgent & ComplianceAgent --> Orchestrator
    Orchestrator --> Output[Asset Allocation Strategy]



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
- Noura Alfaadhel
- Joud Aldawsari
- Reema Alhawassi
- Sarah Al Sa'ed
- Norah Al Juwair

## Program Attribution 
- **Course :** Buliding Agentic AI System 
- **Trainer :** Mohammed Albelad
- **Presented by :** SDAIA Academy 

SDAIA Academy GitHub :
https://github.com/SDAIAAcademy

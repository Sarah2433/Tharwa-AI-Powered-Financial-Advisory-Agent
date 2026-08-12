
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




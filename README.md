
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

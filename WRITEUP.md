1. Agent Fundamentals & LangSmith Observability

This section demonstrates a single tool-calling agent using the core LangChain primitives: two real data tools (get_stock_price, get_company_fundamentals via yfinance) plus a web-search tool (TavilySearch) are bound to gemini-3.5-flash with bind_tools, and the model's chosen tool_calls are designed to then be executed against a tool_map, which would prove the LLM (not hardcoded logic) decided which tool(s) to call — though this has not yet been confirmed by an actual run. A Pydantic schema (InvestmentAnalysis) is also used with with_structured_output to force a constrained buy/hold/sell decision. Gap: LangSmith tracing is only configured via environment variables — no cell prints a run URL or otherwise confirms a trace was actually captured, so "Observability" is set up but not evidenced. Additionally, the saved notebook shows a NameError: llm_with_tools is not defined on the tool-calling cell, meaning this section has not successfully run end-to-end as submitted.

2. RAG Pipeline

The design choice is Agentic RAG, not a fixed two-step retrieve-then-generate chain: search_regulations is exposed as a bound tool so the LLM decides at runtime whether, how many times, and with what phrasing to query the Saudi CMA regulatory corpus. The notebook includes three test queries designed to run against search_regulations.invoke(...) and print the retrieved, source-attributed text — but no output was saved, so this has not been confirmed to actually work. Gap: retrieval quality itself is never scored against a labeled test set.

3. Workflow Pattern — Evaluator-Optimizer

generate_strategy produces an initial allocation from risk profile and horizon; evaluate_compliance (the evaluator) grounds its verdict in real search_regulations retrievals; optimize_strategy (the optimizer) mechanically caps any flagged asset class. The loop is designed to run end-to-end up to MAX_OPTIMIZATION_ROUNDS and print the rounds used and final allocation, but no execution output was saved, so this has not been confirmed. Gap: the demo case never actually triggers a real violation, so the optimizer's correction branch is never shown firing.

4. Context & State Management

A custom InvestorState carries structured investor fields alongside message history. Short-term memory uses an InMemorySaver checkpointer (per-thread); long-term memory uses an InMemoryStore with remember/recall helpers keyed by user_id. The cross-thread test is designed to be the key evidence — recording risk_level on one thread and recalling it from a different one — but no output was saved to confirm it actually succeeded. Gap: the second thread is never actually used to invoke the agent, so a live conversation continuing from recalled state isn't demonstrated.

5. Multi-agent / Routing Architecture

Implements the Supervisor + workers pattern (Track A): three specialized workers (market_analysis_agent, portfolio_agent, compliance_agent) are registered with create_supervisor, which owns the sole routing decision. The intended evidence is a printed transfer_to_<worker> / transfer_back_to_supervisor tool-call sequence, which would prove the LLM chose which specialist to invoke — but no output was saved for this cell either. Gap: state_schema=InvestorState is missing from the workers/supervisor, so shared state fields don't propagate into this subgraph.

6. Human-in-the-loop

An AllocationDecision schema and a human_approval tool built on interrupt() are defined, and an agent is assembled with both record_risk_profile and human_approval as tools. Gap (important): this section never actually invokes the agent to pause and resume — the only place interrupt/resume is demonstrated end-to-end is later, in the Functional API section, using a different hand-rolled interrupt() call rather than this section's own tool.

7. LangGraph Functional API & Error Handling

@task/@entrypoint structure the workflow as composable steps. Two error strategies are shown concretely: retry (RetryPolicy on fetch_market_data_task and flaky_task_demo, designed to force a failure on the first call and print the attempt count before success — not confirmed by a saved run) and fallback (a retrieval task catches exceptions and substitutes a placeholder). Gap: only 2 of the 4 standard error strategies are demonstrated — no circuit-breaker or fail-fast example.

8. SQLite Persistence

SqliteSaver replaces the in-memory checkpointer with a durable one, and an agent using it is designed to record a risk profile under a persistence-test thread, but this has not been confirmed by a saved run either. Gap: persistence is only asserted, not proven — everything runs in one session, with no second cell reopening a new SqliteSaver connection to confirm the data survives a restart.
# Logical Architecture Diagram

The diagram emphasizes Adara's three architectural themes: tick-driven market ingestion, snapshot/live portfolio state, and retained decision provenance.

```mermaid
flowchart TB
    providers["Public market-data providers"]
    exchanges["Digital-asset exchanges"]
    aiService["OpenAI API"]

    subgraph adara["ADARA — Public logical responsibilities"]
        console["Web Console"]
        aially["AiAlly / AI-Assisted Interaction"]
        market["Tick-driven Market Data & Normalization"]
        live["Live External State Refresh"]
        snapshot["Operational Portfolio Snapshot Layer"]
        portfolio["Portfolio & Valuation"]
        strategies["Strategy Lifecycle"]
        trading["Trading & Order Management"]
        compliance["Pre-trade Compliance"]
        decision["Retained Decision Context"]
        state["Persistent Operational State (MySQL)"]
        analysis["Analysis & Reporting"]
        admin["Administration & Notifications"]
        tools["Curated MCP Capability Boundary"]

        market -->|"normalized ticks"| strategies
        market -->|"valuation context"| portfolio
        live -->|"fresh external state"| portfolio
        snapshot -->|"materialized state"| portfolio
        portfolio -->|"portfolio context"| strategies
        portfolio -->|"exposure / valuation"| compliance
        strategies -->|"automated intent"| trading
        console -->|"discretionary intent"| trading
        trading -->|"validated intent"| compliance
        compliance -->|"eligible order"| trading
        trading -->|"order / execution state"| decision
        portfolio -->|"time-relevant context"| decision
        compliance -->|"evaluation context"| decision
        decision --> state
        portfolio --> state
        state --> analysis
        analysis --> admin
        console --> aially
        aiService -->|"remote MCP requests"| tools
        tools -->|"selected capability results"| aiService
        aially -->|"Responses request"| aiService
        aiService -->|"streamed response"| aially
    end

    providers -->|"streaming market data"| market
    exchanges -->|"streaming market data"| market
    exchanges -->|"balances / account state"| live
    snapshot -.->|"periodic acquisition"| live
    trading -->|"orders that pass controls"| exchanges
    exchanges -->|"execution / fills"| trading
```

This is a responsibility view, not a deployment or module map. Multiple responsibilities may live within the same runtime component.

The snapshot and live-refresh paths are intentionally shown as separate concepts because freshness and external-dependency cost are architectural choices.

AiAlly is a later interface layer and does not own trading decisions or deterministic compliance controls.

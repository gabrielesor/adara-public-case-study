# System Architecture

Adara is a modular Java trading and investment-operations platform built around three cross-cutting architectural principles:

1. **Robustness by design**
2. **Tick-driven by design**
3. **Decision provenance by design**

Those principles are more useful for understanding the system than a one-to-one list of JARs or runtime processes.

## Architectural perspective

Adara coordinates several state domains with very different timing characteristics.

Market prices can change continuously and arrive as individual updates. Portfolio state spans multiple accounts and external systems. Human traders and automated strategies create intent. Compliance must evaluate that intent before execution. Exchange state then has to be reconciled back into persistent history. Years later, the platform should still be able to explain how an order entered the system and what operational context surrounded it.

The architecture therefore separates a latency-sensitive market-data path from slower aggregate-state, reporting, notification, and historical-evidence concerns.

## Public logical responsibilities

### Web Console

The Web Console is the operational user interface for portfolio supervision, discretionary trading, strategy management, order review, compliance, reporting, and administration.

### Market Data & Normalization

This area receives streaming market data from exchanges and other public market-data sources.

A fundamental design choice is that the live path is **tick-driven**: a tick is an individual pair-price update. The system can process those updates in memory as they arrive rather than waiting for a candle or fixed-interval polling cycle.

Exchange/provider-specific identifiers are mapped into a canonical internal asset and pair model.

### Operational Portfolio Snapshot Layer

Adara may represent many accounts and holdings across multiple external systems. Rebuilding aggregate fund state by synchronously interrogating every remote account for every dashboard view or control would introduce unnecessary latency and dependency coupling.

An independent process therefore materializes portfolio state periodically into persistent and in-memory operational snapshots. Those snapshots can serve dashboard and applicable compliance reads efficiently.

Where freshness takes priority, a live path can bypass the snapshot and reacquire current external state.

This is an architectural trade-off between **freshness, latency, and dependence on remote APIs**, not a hidden cache optimization.

### Portfolio & Valuation

Portfolio responsibilities combine balances, valuations, composition, and exposure at account and aggregate level. Heterogeneous values are normalized to a common reference basis for controls and reporting.

### Strategy Lifecycle

Automated strategies consume current market state and produce trading intent. They do not own a separate ungoverned execution route: their intent enters the same broader validation, compliance, order-management, and exchange-execution path as other Adara-originated orders.

### Trading & Order Management

This area accepts discretionary and automated intent and manages validation, submission, monitoring, fills, cancellation, fees, history, and provenance.

The V1 order path contains a known architectural debt: some report-generation and notification side effects remained synchronous, so observed end-to-end order creation could reach roughly **2–8 seconds**. This is documented explicitly because it is one of the V2 drivers.

### Compliance

Compliance evaluates applicable controls before Adara-originated exchange submission. It uses order and portfolio context and can block an order before the exchange boundary.

### Decision Context & Persistent State

MySQL persistence is not only a store of final orders.

Adara retains operational state needed to review an order later: origin, relevant account/portfolio state, strategy context, compliance evidence, and lifecycle information. The design goal is practical reconstructability of historical automated and controlled activity.

### Analysis, Reporting & Notifications

Retained operational state supports historical analysis, compliance reporting, order-level evidence, scheduled portfolio reports, and operational notifications.

### AiAlly / AI-Assisted Interaction

AiAlly is a later interface layer. Its current replacement architecture uses the OpenAI Responses API, remote MCP, and optional document retrieval. It does not own the deterministic trading, portfolio, or compliance rules underneath it.

## Controlled execution path

At architecture level:

**market tick → normalized market state → portfolio / strategy context → trading intent → validation → compliance → exchange submission → execution monitoring → retained order and decision context**

A discretionary order and an automated-strategy order share the controlled path. Externally originated exchange activity can later be synchronized and retained, but its provenance remains external.

## Fast path vs. side effects

One of the most useful lessons from Adara V1 is that not all work belongs on the same synchronous path.

The market-data path was designed with strong sensitivity to latency because some strategies evaluated every incoming price movement.

The V1 order-creation path accumulated additional synchronous responsibilities over time, including PDF/report generation and email/notification activity. That coupling is operationally functional but inefficient for fast execution.

The V2 architecture is therefore being designed around stricter separation of:

- fast decision/execution work;
- durable state changes;
- asynchronous audit/report generation; and
- notification side effects.

## System context and diagrams

- [System Context](../diagrams/system-context.md)
- [Logical Architecture](../diagrams/logical-architecture.md)
- [Market State & Snapshot Path](../diagrams/market-state-and-snapshots.md)
- [Order Lifecycle](../diagrams/order-lifecycle.md)

## Technology view

At a high level:

- Java backend;
- MySQL relational persistence;
- AWS deployment;
- direct exchange APIs;
- streaming/WebSocket market-data integration;
- in-memory operational state;
- scheduled/background processing; and
- modular application architecture.

The public case study deliberately omits credentials, private endpoints, exact deployment topology, proprietary strategy algorithms, and security-sensitive configuration.

[← Previous](03-capability-map.md) | [Case Study Home](../README.md) | [Next →](05-market-data-and-portfolio.md)

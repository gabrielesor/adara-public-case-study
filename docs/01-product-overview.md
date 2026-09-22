# Product Overview

Adara is a proprietary production platform for digital-asset trading and investment operations. It coordinates live market data, portfolio and account state, discretionary and automated trading, direct exchange execution, order management, pre-trade compliance, reporting, analytics, administration, and monitoring in one operating environment.

It has been used in production since June 2023. This is important context: Adara is not a design exercise or an isolated algorithm. It is a long-running system that has processed real orders through direct exchange integrations and retained the operational evidence needed to review those activities later.

## The problem Adara solves

A real trading operation has to coordinate several forms of state that change at very different speeds.

Market prices can change many times per second. Exchange APIs expose heterogeneous identifiers, constraints, and failure modes. Portfolio state may be distributed across many accounts. Human traders and automated strategies can both create trading intent. Before an Adara-originated order reaches an exchange, the system must validate it and apply applicable compliance controls. After submission, it must monitor execution, retain fills and fees, preserve origin, and keep enough historical context to understand the order later.

Adara was built to connect these responsibilities rather than implement them as unrelated scripts.

## Three founding architectural principles

### Robustness by design

External exchanges, market-data feeds, networks, application processes, and notification channels are treated as failure-prone boundaries. The architecture therefore emphasizes state retention, separation of operational surfaces, monitoring, reconnect/recovery behavior, and the ability to continue or restore unattended operation without relying on a user keeping a browser session alive.

Robustness was present in the early architecture work before production and remains a first-class concern. See [Operations and Reliability](09-operations-and-reliability.md).

### Tick-driven by design

Adara's real-time market path was designed around incoming price updates rather than fixed-interval candles. A *tick* in this case study means an individual pair-price update received from an exchange or market-data integration.

That decision shaped the ingestion and in-memory processing model because some automated strategies needed to react to price movement as it arrived. One retained nine-month corpus contains **120M+ tick-level observations**. The figure is intentionally described as a retained corpus, not as a lifetime count of every tick handled by the live system.

See [Market Data and Portfolio](05-market-data-and-portfolio.md).

### Decision provenance by design

A historical order should not become an unexplained database row. Adara retains origin and time-relevant operational context so that a later review can reconstruct why a controlled order was allowed and, for automated activity, why the strategy generated it.

The design links orders to retained account/portfolio state, strategy origin and configuration context, and compliance evidence. This enables practical reconstructability even years after the event.

See [Order and Trading Lifecycle](06-order-and-trading-lifecycle.md) and [Compliance and Audit](07-compliance-and-audit.md).

## Operational model

Adara integrates directly with digital-asset exchanges, including Kraken and Binance, and with public market-data sources. External asset identifiers are normalized into a canonical internal representation so the rest of the platform can reason consistently about assets, pairs, balances, valuations, orders, and reports.

The platform has supported up to roughly fifteen represented accounts across exchange and portfolio roles. Continuously rebuilding aggregate fund state by synchronously querying every remote account would make dashboard and control paths unnecessarily dependent on external latency and availability.

Adara therefore uses an operational snapshot model: an independent process periodically materializes aggregate portfolio state into persistent and in-memory form. Dashboard and applicable compliance reads can use that state efficiently, while a configurable live-refresh path can bypass the snapshot and reacquire current external state when freshness takes priority.

## Trading and execution

Human-directed trading and automated strategies use the same broader operational infrastructure. Both create Adara-originated intent that can proceed through validation, pre-trade compliance, exchange submission, execution monitoring, persistence, and analysis.

Automated strategies consume real-time market state and can maintain their own lifecycle and configuration. Proprietary decision logic remains outside this public case study; what is public is the surrounding engineering required to run strategies safely inside a production platform.

## Compliance as execution control

Compliance is not only a report produced after trading. Applicable controls are evaluated before Adara-originated exchange submission. Controls can use normalized order value, account exposure, portfolio exposure, concentration, and other retained operating context.

A failed applicable control stops the Adara-originated order before the exchange boundary. Successful controlled orders can be associated with retained order-level compliance evidence, while scheduled portfolio-level reporting supplies a separate historical view.

## Production evidence

As of September 2026, the public case study uses the following aggregate evidence:

- production operation since **June 2023**;
- **5,000+** production orders processed;
- **€18M+** aggregate traded volume through Adara-supported workflows;
- **120M+** retained tick-level observations in one nine-month corpus;
- **50+** operational capabilities; and
- a private source history spanning **five years and 538 commits**.

These figures establish production use and engineering scale. They are not claims about investment performance, profitability, availability percentage, or a lifetime tick total.

## Technical positioning

At a high level, Adara uses a modular Java backend, MySQL relational persistence, AWS deployment, streaming/WebSocket integration, in-memory state, scheduled/background processing, and direct exchange APIs.

The public architecture intentionally omits credentials, private endpoints, exact infrastructure topology, proprietary strategy algorithms, and real financial/account data.

[← Previous](../README.md) | [Case Study Home](../README.md) | [Next →](02-role-and-history.md)

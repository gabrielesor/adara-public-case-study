# Adara — Production Trading & Investment Operations Platform

**Public technical case study of a proprietary system designed, built, and operated in production since June 2023.**

Adara is a proprietary production trading and investment-operations platform, used as internal tooling in a real investment-management environment since June 2023. Gabriele Soranzo conceived the product, designed its architecture, built the original platform end-to-end, and has remained responsible for its technical evolution and production operation.

## Production evidence at a glance

| Dimension | Public evidence |
| --- | --- |
| Production operation | **Since June 2023** |
| Order processing | **5,000+ production orders** |
| Retained market-data evidence | **120M+ tick-level observations in one retained nine-month corpus** |
| Product breadth | **50+ operational capabilities** |
| Known individual users | **~10 across Fund Manager, Trader, Guest, and Admin roles** |
| Private source history | **Five-year development history; 538 commits as of September 2026** |

The 120M+ figure is deliberately narrow: it describes one retained nine-month market-data corpus that can be directly evidenced. It is **not** presented as a lifetime count of every tick processed by Adara in real time.

The user figure is also deliberately conservative: it refers to approximately ten identifiable individuals who have used Adara over its production history. Account counts varied over time because some people held more than one role.

## Four architectural principles

### Robustness by design

Robustness was a design requirement from the beginning, not an operational feature added after deployment. Exchange connectivity, market-data ingestion, persistent operational state, background processing, monitoring, and recovery were designed around the assumption that external systems, network connections, processes, and individual runtime paths can fail.

The production architecture therefore separates operational surfaces, retains state across process boundaries, monitors externally visible health independently, and treats exchange/data-provider connectivity as a failure-prone integration boundary. See [Operations and Reliability](docs/09-operations-and-reliability.md).

### Tick-driven by design

Adara was designed around **individual price updates**, not around periodic candle polling as its primary real-time abstraction. In this case study, a *tick* means an individual pair-price update received from an exchange or market-data integration.

Streaming ingestion and low-latency in-memory handling were first-class concerns because automated strategies could react to changing prices as they arrived. One retained nine-month corpus contains more than 120 million tick-level observations. See [Market Data and Portfolio](docs/05-market-data-and-portfolio.md).

### Compliance by design

Compliance is part of the execution path, not only a report generated afterwards.

For Adara-originated discretionary and automated activity, applicable compliance controls are evaluated before exchange submission. A failing enforced control stops the order before it crosses the exchange boundary. The same architecture also retains order-level and portfolio-level evidence for later review.

See [Compliance, Evidence and Reconstructability](docs/07-compliance-and-audit.md).

### Decision provenance by design

For an automated financial system, retaining the final order is not enough. Adara was designed so that a historical order can be examined together with the operational context in which the decision and compliance evaluation were made.

The persistent model links order history to time-relevant portfolio/account state, strategy origin, and compliance context. The objective is practical reconstructability: years later, the system should still be able to answer questions such as **“Why was this order allowed?”** and, for automated activity, **“What state caused the strategy to act?”**

See [Order and Trading Lifecycle](docs/06-order-and-trading-lifecycle.md) and [Compliance, Evidence and Reconstructability](docs/07-compliance-and-audit.md).

## What Adara is

Adara brings live market data, multi-account portfolio state, discretionary and automated trading, direct exchange execution, order management, pre-trade compliance, reporting, analytics, administration, and monitoring into one production platform.

It integrates directly with digital-asset exchanges, including Kraken and Binance, through API and streaming/WebSocket capabilities. Exchange-specific asset identifiers are normalized into a canonical internal model. Portfolio values and exposures are normalized into a common reference basis for controls and reporting.

Adara is therefore broader than a trading bot. It coordinates the path from changing market state and trading intent through validation, compliance, execution, persistence, evidence, and later analysis.

## My role

I conceived Adara and designed its original architecture. I initially implemented the platform end-to-end: the Java backend, relational model, exchange connectivity, market-data processing, portfolio model, order lifecycle, automated-strategy hosting, compliance workflows, reporting, operational tooling, and AWS production deployment.

A junior developer later contributed to selected areas under my technical direction, especially user-interface work. I have remained responsible for architecture, integration decisions, production operation, and the subsequent AI/MCP evolution.

See [Product Ownership and Engineering Role](docs/02-role-and-history.md).

## Architecture in one view

At a high level, Adara combines:

- **streaming market-data ingestion and normalization**;
- **low-latency in-memory tick handling** for real-time strategy evaluation;
- a **persistent/in-memory operational portfolio snapshot layer** to avoid rebuilding aggregate state from many remote accounts on every read;
- a **live-refresh path** when fresh exchange state must bypass the snapshot;
- **discretionary and automated trading intent**;
- **pre-trade compliance** before Adara-originated exchange submission;
- **order, strategy, and decision provenance** retained in MySQL;
- **reporting, notifications, analytics, monitoring, and scheduled processing**; and
- a later **AI-assisted interface (AiAlly)** built on the OpenAI Responses API, remote MCP, and document retrieval.

See [System Architecture](docs/04-system-architecture.md) and the [Market State & Snapshot Diagram](diagrams/market-state-and-snapshots.md).

## A real V1 trade-off

The market-data path was designed with strong latency awareness. The V1 order-creation path evolved differently: some report-generation and notification work remained synchronous, so observed end-to-end order creation could reach roughly **2–8 seconds**.

That is documented here deliberately. It is a real architectural trade-off and accumulated technical debt, not something hidden behind a “perfect architecture” narrative. It is also one of the drivers for the Adara V2 architecture, where decisioning/execution and audit/reporting side effects are being separated more aggressively.

## Source provenance

This public case-study repository is intentionally recent. It is documentation, not the proprietary application repository.

The underlying Adara application source remains private and has a **five-year development history with 538 commits as of September 2026**. Keeping the application repository private protects proprietary strategy logic, security-sensitive configuration, and production implementation details while this repository exposes the architecture, engineering decisions, operating evidence, and lessons learned.

## AI usage and authorship

Most of Adara's core trading, portfolio, compliance, order-management, and operational architecture was designed and implemented **before generative AI became part of its development workflow**.

Generative AI has been used in bounded later work:

- design and implementation support for **AiAlly v2**, the Responses API / remote MCP / RAG generation;
- development assistance used by the junior contributor, especially in UI work; and
- creation and refinement of this public technical case study.

The core production platform should therefore not be interpreted as an AI-generated system. AiAlly is a later interface layer over selected Adara knowledge and capabilities; it is not the system that makes proprietary trading decisions. See [AI-Assisted Operations — AiAlly](docs/11-ai-assisted-operations.md).

## Explore the case study

1. [Product overview and architectural principles](docs/01-product-overview.md)
2. [Product ownership, engineering role, source provenance, and AI authorship](docs/02-role-and-history.md)
3. [Capability map](docs/03-capability-map.md)
4. [System architecture](docs/04-system-architecture.md)
5. [Tick-driven market data, portfolio state, and snapshots](docs/05-market-data-and-portfolio.md)
6. [Order lifecycle and decision provenance](docs/06-order-and-trading-lifecycle.md)
7. [Compliance, evidence and reconstructability](docs/07-compliance-and-audit.md)
8. [Strategy platform and latency-sensitive experiments](docs/08-strategy-platform.md)
9. [Robustness and production reliability](docs/09-operations-and-reliability.md)
10. [Operational scale](docs/10-operational-scale.md)
11. [AI-assisted operations with AiAlly](docs/11-ai-assisted-operations.md)

## Confidentiality and scope

This repository documents architecture and production experience without publishing proprietary application source code, credentials, exact strategy algorithms, real account identifiers, positions, balances, or transaction-level financial data. Screenshots and historical artifacts must use synthetic, sanitized, or non-sensitive material.

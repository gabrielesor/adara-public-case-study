# Adara — Public Technical Case Study

Adara is a proprietary trading and investment-operations platform owned by Gabriele Soranzo. He conceived the product, designed its original architecture, and initially developed it end-to-end. Adara has operated in production as internal tooling in a real investment-management environment since June 2023.

## Production at a glance

- **In production since:** June 2023
- **Production orders processed:** 5,000+
- **Operational capabilities:** 50+
- **Trading modes:** discretionary and automated
- **Compliance:** pre-trade enforcement

## What Adara is

Adara brings real-time market data, portfolio and account state, trading, order management, compliance, reporting, and operational analytics into one production platform. It supports both human-directed activity and automated strategies, including execution through direct integrations with digital-asset exchanges.

The platform ingests streaming data from external public market-data providers and normalizes exchange-specific asset identifiers into a canonical internal model. It maintains balances, valuations, and exposure at account and portfolio level, with heterogeneous trading values normalized into a common reference currency for controls and reporting.

Adara is therefore broader than a trading algorithm: it coordinates the operational path from changing market state and a trading decision through validation, compliance, execution, persistence, and subsequent analysis.

AiAlly adds an AI-assisted natural-language interface for product knowledge and selected operational context. Its first OpenAI Assistants API integration is a historical production implementation that is currently unavailable following retirement of the upstream API; the replacement based on the OpenAI Responses API and remote MCP has been successfully validated in pre-production, with production rollout pending. See [AI-Assisted Operations — AiAlly](docs/11-ai-assisted-operations.md).

## My role

Gabriele Soranzo conceived Adara, designed its original architecture, and initially developed the platform end-to-end. A junior developer later contributed to selected development activities under his technical direction.

## What the platform does

Adara's 50+ operational capabilities are organized into eight public domains:

- **Market Data** — streaming ingestion and normalization of external market data.
- **Portfolio** — multi-account state, balances, valuations, exposure, and concentration views.
- **Trading** — discretionary and automated execution, including exchange-specific capabilities.
- **Orders** — validation, submission, monitoring, fill tracking, cancellation, fees, provenance, and history.
- **Strategies** — configuration, execution, and monitoring of automated trading strategies.
- **Compliance** — pre-trade controls applied to Adara-originated discretionary and automated orders.
- **Analysis & Reporting** — operational analytics, historical analysis, and compliance reporting.
- **Administration & Operations** — capabilities supporting administration, scheduled operation, monitoring, notifications, and AI-assisted interaction.

## High-level architecture

Adara uses a modular Java backend, MySQL relational persistence, and AWS deployment. Streaming connectivity integrates external market-data providers and digital-asset exchanges, while persistent operational state supports the platform's workflows. See [System Architecture](docs/04-system-architecture.md) for the public architecture view; detailed deployment topology is intentionally outside the scope of this case study.

## A production order lifecycle

An Adara-originated production order follows a common high-level path:

**Trader or strategy → validation → compliance evaluation → exchange submission → monitoring and execution → persistence, reporting, and analytics**

The lifecycle distinguishes discretionary, automated, and externally originated activity while retaining order provenance. See [Order and Trading Lifecycle](docs/06-order-and-trading-lifecycle.md).

## Compliance by design

Compliance evaluation is part of the pre-trade path for Adara-originated orders. Applicable controls—covering categories such as trade size, account exposure, portfolio exposure, and portfolio concentration—are evaluated before exchange submission. An Adara-originated order that fails those controls is prevented from reaching the exchange.

The same compliance layer applies to Adara-originated discretionary orders and automated-strategy orders. For an order that successfully follows this controlled path, Adara can produce an order-level compliance PDF; the separate daily portfolio-level compliance PDF provides a scheduled portfolio view. These reports are distributed by email and retained server-side. See [Compliance and Audit](docs/07-compliance-and-audit.md).

## Operational experience

Adara has operated in production since June 2023 and has processed more than 5,000 production orders. Its production web console is monitored through an external uptime-monitoring service. This case study does not claim uninterrupted availability or publish an overall availability percentage.

See [Operations and Reliability](docs/09-operations-and-reliability.md) and [Operational Scale](docs/10-operational-scale.md).

## Technology overview

At a high level, the platform uses a Java backend, MySQL relational persistence, AWS deployment, modular architecture, streaming connectivity, and persistent operational state. Low-level deployment topology and specific AWS services are intentionally outside the public scope.

## Explore the case study

1. [Understand the product and its operational context](docs/01-product-overview.md)
2. [Review product ownership and development history](docs/02-role-and-history.md)
3. [Explore the platform capability map](docs/03-capability-map.md)
4. [Examine the public system architecture](docs/04-system-architecture.md)
5. [Follow market data and portfolio state](docs/05-market-data-and-portfolio.md)
6. [Trace the order and trading lifecycle](docs/06-order-and-trading-lifecycle.md)
7. [Review compliance controls and auditability](docs/07-compliance-and-audit.md)
8. [Understand the strategy-platform boundary](docs/08-strategy-platform.md)
9. [Review production operations and reliability](docs/09-operations-and-reliability.md)
10. [Examine disclosed operational scale](docs/10-operational-scale.md)
11. [Understand AI-assisted operations with AiAlly](docs/11-ai-assisted-operations.md)

## Confidentiality and scope

This repository is a public technical case study. Proprietary application source code, organisation, fund, and stakeholder identities, real positions, balances, and transaction data are intentionally excluded. Proprietary trading decision logic is also outside the public scope. Any screenshots published here must use synthetic or properly sanitized data.

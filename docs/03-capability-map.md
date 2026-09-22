# Capability Map

Adara contains more than 50 operational capabilities spanning live market data, multi-account portfolio state, trading, automated strategies, controlled execution, compliance, reporting, and production operations.

This page is a map of responsibilities rather than a menu inventory. The architectural story behind those capabilities is driven by three cross-cutting principles: **robustness by design, tick-driven by design, and decision provenance by design**.

## Market Data

The Market Data domain brings continuously changing external market state into Adara.

Representative capabilities include:

- streaming and real-time exchange market data;
- individual tick / pair-price update handling;
- public market-data-provider integration;
- canonical asset and pair normalization;
- historical market-data retention and analysis; and
- derived market information used by portfolio and strategy logic.

The real-time path is designed around incoming price updates rather than periodic candle polling as its primary abstraction.

## Portfolio

The Portfolio domain maintains account and aggregate portfolio state used by trading, compliance, reporting, and analysis.

Representative capabilities include:

- multiple account roles across exchanges and external holdings;
- balances and valuations;
- account-level and portfolio-level exposure;
- portfolio composition and historical state;
- common-reference-value normalization; and
- a periodically materialized operational snapshot used to avoid synchronously rebuilding the entire portfolio from remote sources on every read.

A live-refresh path can bypass the snapshot when current external state is required.

## Trading

Trading supports both human-directed and automated intent.

Representative capabilities include:

- discretionary trading;
- automated trading;
- validation before submission;
- direct exchange execution;
- exchange-specific order capabilities; and
- common integration with portfolio state, compliance, order management, and provenance.

## Orders

The Orders domain manages an order as a lifecycle, not as a single REST call.

Representative capabilities include:

- order construction and validation;
- pre-trade compliance;
- exchange submission;
- status and fill monitoring;
- cancellation and fee handling;
- history and analytics;
- origin/provenance; and
- association with the time-relevant operational context used to explain historical decisions.

The platform distinguishes Adara-originated discretionary and automated orders from activity that originated directly at an exchange.

## Strategies

Adara hosts automated strategies inside the wider operational platform.

Representative capabilities include:

- strategy configuration and lifecycle;
- real-time evaluation against tick-driven market state;
- automated order generation;
- instance-level monitoring;
- retained activity and historical analysis; and
- shared validation, compliance, order-management, and execution infrastructure.

Proprietary decision logic remains outside this public case study.

## Compliance

Compliance is part of the controlled pre-trade path for Adara-originated orders.

Representative capabilities include:

- single-trade-size controls;
- account exposure;
- overall portfolio exposure;
- portfolio or asset-category concentration;
- normalized-value evaluation;
- early-warning thresholds;
- multi-channel notifications;
- order-level compliance evidence; and
- scheduled portfolio-level compliance reporting.

If an applicable enforced control fails, the Adara-originated order is stopped before exchange submission.

## Analysis & Reporting

Retained operational state supports:

- portfolio analytics and composition;
- exposure analysis;
- order and fill history;
- pair and trading analysis;
- strategy activity analysis;
- compliance review; and
- historical reconstruction of the operating context associated with an order.

## Administration & Operations

The operating platform includes:

- users, roles, and access administration;
- settings and configuration;
- scheduled processing;
- notifications;
- external uptime monitoring;
- production diagnostics;
- persistent operational state; and
- AI-assisted interaction through AiAlly.

## Cross-cutting workflow

At a high level:

**Tick / market state → operational portfolio state → human or automated intent → validation → compliance → exchange execution → retained order + decision context → reporting and analysis**

The value of Adara comes from the integration of those responsibilities. Market data is not an isolated feed; portfolio state is not a passive dashboard; compliance is not only post-trade reporting; and automated strategies are not detached scripts with their own ungoverned route to an exchange.

[← Previous](02-role-and-history.md) | [Case Study Home](../README.md) | [Next →](04-system-architecture.md)

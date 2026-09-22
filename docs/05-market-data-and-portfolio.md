# Market Data and Portfolio

Adara's market-data architecture starts from a simple premise: **the market changes as individual price updates arrive**.

That premise shaped both the real-time ingestion path and the way automated strategies consume market state.

## Tick-driven by design

In this case study, a *tick* means an individual price update for a trading pair received from an exchange or market-data integration.

Adara was designed to receive and process those updates continuously, with low-latency in-memory handling available to strategy logic. The primary real-time abstraction is therefore not “wait for the next one-minute candle”; it is “react to the next relevant price update.”

This mattered especially for strategies whose behavior depends on following price movement closely.

### Retained data evidence

One retained market-data corpus spans approximately **nine months** and contains more than **120 million tick-level observations**.

This number is intentionally conservative and precisely scoped. It is evidence of the volume of tick data that Adara can retain and process, but it is **not presented as a lifetime total of all ticks processed by the live platform**.

Real-time processing and long-term retention are different responsibilities. The live system can consume a tick without requiring that every such event be kept indefinitely.

## Streaming integration and normalization

Adara integrates with digital-asset exchanges and public market-data sources.

External venues can identify the same economic asset differently. Adara therefore maps source-specific identifiers into a canonical internal representation used consistently by portfolio, trading, compliance, reporting, and analytics.

Canonical normalization does not remove exchange-specific capabilities; it prevents provider-specific naming from leaking unnecessarily through the rest of the system.

## Multi-account portfolio model

Adara represents multiple account roles, including trading-enabled, read-only, externally managed, and reference/aggregation roles.

At peak operating breadth, the platform represented roughly fifteen accounts across Kraken and Binance-related workflows and other portfolio roles.

Account-level state is preserved while aggregate portfolio state can be calculated for supervision, exposure, compliance, and reporting.

## Operational Portfolio Snapshot Layer

A direct live query against every remote account is not always the right way to answer a dashboard or compliance question.

Repeatedly rebuilding the complete portfolio from many exchange APIs would couple user-facing and control paths to external latency, rate limits, and transient availability.

Adara therefore uses an independent snapshot process that periodically materializes fund state into:

- **persistent operational state**, and
- **in-memory state** used by fast read paths.

The normal snapshot cadence is approximately hourly.

Dashboard and applicable compliance logic can use this materialized state when that freshness model is appropriate.

## Live-refresh bypass

Snapshot use is not mandatory.

The user or configuration can force a path that reacquires live external state before the calculation proceeds. That gives the system two explicit operating modes:

**snapshot path → lower latency / lower external dependency**

**live-refresh path → greater freshness / higher external dependency and latency**

The important design point is that this is a controlled freshness choice, not an accidental stale-cache behavior.

## Valuation and exposure

Balances and trading values can be expressed through heterogeneous assets and pairs. Adara normalizes relevant values to a common reference basis so portfolio valuation, exposure, compliance, and reporting can reason consistently.

Exposure exists both at account and aggregate portfolio level. Asset classification and composition can contribute further context to compliance and analysis.

## Relationship to automated strategies

Automated strategies may consume the live tick stream directly.

One production strategy family used Adara's own price-following logic rather than delegating all behavior to native exchange stop-loss, take-profit, or trigger facilities. That required Adara to maintain continuous awareness of market state and make its own decisions as prices evolved.

A separate triangular-arbitrage strategy was tested against a real Kraken environment but was not promoted to production because the economic results were not sufficiently compelling. It remains useful as an engineering example because millisecond-scale timing materially affected whether an apparent opportunity still existed by the time execution could occur.

The proprietary strategy algorithms are not published here.

## Historical state

Adara retains historical market and portfolio information to support analysis and decision provenance.

The design distinguishes:

- live current state;
- operational snapshots;
- longer-lived historical state; and
- order-linked decision/compliance context.

Those categories serve different purposes and should not be collapsed into one “cache” concept.

## Public boundary

This page does not expose real account identifiers, credentials, balances, positions, proprietary strategy formulas, private endpoints, or detailed exchange configuration. It documents the architectural treatment of market events, account aggregation, freshness, and state retention.

[← Previous](04-system-architecture.md) | [Case Study Home](../README.md) | [Next →](06-order-and-trading-lifecycle.md)

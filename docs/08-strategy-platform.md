# Strategy Platform

Adara hosts automated strategies inside the same production platform that owns market data, portfolio state, compliance, order management, exchange connectivity, persistence, and analysis.

A strategy is therefore not a detached bot with its own private route to an exchange.

## Strategy lifecycle

At capability level, Adara supports:

- strategy configuration;
- instance creation/initialization;
- activation and deactivation;
- real-time evaluation;
- generated trading intent;
- instance monitoring;
- order association; and
- historical analysis.

Multiple strategy instances can coexist while remaining individually identifiable and reviewable.

## Real-time market participation

Strategies can consume the tick-driven market-data path.

This is important because some strategy behavior is defined by continuous price movement rather than periodic batch evaluation.

One strategy family implemented Adara-side **price following**: Adara itself maintained the changing execution logic using live market state rather than delegating the complete behavior to native exchange stop-loss, take-profit, or trigger orders.

The exact algorithm is proprietary and is not published.

## Latency-sensitive experimentation

Adara also included a triangular-arbitrage strategy tested against a real Kraken environment.

The strategy was not promoted to production because testing did not demonstrate economic results strong enough to justify operating it.

Architecturally, however, it was an important experiment: apparent opportunities could disappear on a millisecond-scale horizon, making market-data processing and execution latency materially relevant.

This is included as an engineering lesson rather than presented as a trading success.

## From strategy decision to controlled order

A hosted strategy produces trading intent.

That intent then enters the same wider path as other Adara-originated orders:

**strategy decision → intent → validation → applicable compliance → exchange submission → monitoring → retained order and decision context**

An automated origin does not bypass compliance, order management, or provenance.

## Decision provenance

A later review should be able to associate an automated order with its strategy origin and the retained operating context relevant to that decision.

The public case study does not reveal signal formulas, but it does document the surrounding architecture that makes strategy activity reviewable after execution.

## Monitoring and analysis

The platform can expose:

- strategy instance state;
- generated activity;
- associated orders;
- aggregate and detail views; and
- retained historical analysis.

That observability is separate from the strategy's proprietary signal generation.

## Production use

Automated strategy execution has been part of Adara's production operation.

The production statement concerns the hosting and execution platform. It is not a statement about investment performance, expected returns, or uninterrupted strategy availability.

## Public boundary

Proprietary strategy names, formulas, thresholds, indicators, parameters, signal logic, capital-allocation rules, and position-management algorithms remain private.

[← Previous](07-compliance-and-audit.md) | [Case Study Home](../README.md) | [Next →](09-operations-and-reliability.md)

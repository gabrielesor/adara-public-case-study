# Compliance, Evidence and Reconstructability

Adara treats compliance as part of controlled execution **and** as a historical evidence problem.

The question is not only “did the order pass?” It is also: **can the system explain later why it passed, using the operational context that existed at the time?**

## Pre-trade enforcement

For Adara-originated discretionary and automated orders, applicable controls are evaluated before exchange submission.

Controls can use context such as:

- normalized order value;
- account state and exposure;
- aggregate portfolio exposure;
- portfolio concentration; and
- asset classification.

If an enforced applicable control fails, the order is stopped before the exchange boundary.

## Warning thresholds vs enforced limits

Adara distinguishes early-warning thresholds from enforced limits.

A warning can trigger operational attention before a formal limit is breached. It does not necessarily mean that the order is non-compliant.

An enforced limit determines whether the controlled path may continue.

Keeping the two concepts separate supports prevention without conflating “attention required” with “execution forbidden.”

## Decision provenance and compliance

Compliance evaluation is linked to retained operational context rather than treated as an ephemeral calculation.

The system is designed to preserve enough context to review a historical order together with the state used for the decision. That can include the relevant account/portfolio snapshot, normalized values, origin, and associated compliance evidence.

The design goal is practical reconstructability of questions such as:

**Why was this order considered compliant on that date?**

This is not a claim of cryptographic immutability or formal legal non-repudiation. It is an engineering property of the persistence model and evidence lifecycle.

## Order-level evidence

For a successfully controlled Adara-originated order, Adara can generate an order-level compliance PDF.

The report is distributed by email and retained server-side. It provides a human-readable evidence artifact associated with the controlled order path.

The report complements the persisted operational state; it is not the only place where the decision context exists.

## Portfolio-level evidence

Adara also produces scheduled portfolio-level compliance reporting.

This is distinct from individual order evidence. It records a portfolio compliance view on a schedule and is retained/distributed separately.

## Snapshot freshness and compliance

Applicable compliance reads can use the operational portfolio snapshot when that freshness model is appropriate.

Because snapshot age and live external state are different concepts, Adara also provides a configurable live-refresh path that can bypass the snapshot and reacquire current external state before evaluation.

This makes freshness an explicit operating choice rather than an implicit side effect.

## Externally originated activity

Orders created outside Adara may later be synchronized into its operational history.

Their presence does not mean Adara performed pre-trade validation or compliance before those orders reached the exchange.

Provenance preserves that distinction.

## Compliance flow

See [Compliance Flow Diagram](../diagrams/compliance-flow.md).

## Public boundary

This case study does not publish rule identifiers, exact thresholds, formulas, real portfolio values, recipient lists, report contents, or confidential stakeholder data. It also does not claim regulatory certification or formal audit assurance.

[← Previous](06-order-and-trading-lifecycle.md) | [Case Study Home](../README.md) | [Next →](08-strategy-platform.md)

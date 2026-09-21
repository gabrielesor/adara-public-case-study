# Order and Trading Lifecycle

Adara coordinates more than the act of sending an API request to an exchange. For trading activity originated through the platform, it connects trading intent, operational context, validation, pre-trade compliance, any applicable human confirmation, exchange submission, execution monitoring, persistent state, compliance evidence, and analytics.

The platform can also observe and retain exchange activity that originated outside Adara. Preserving that distinction is central to the public lifecycle: orders created through Adara follow its controlled pre-trade path, while externally originated activity can enter the operational history through synchronization without being represented as previously controlled by Adara.

## Scope

This page describes the public responsibility-level lifecycle for discretionary and automated orders originated through Adara, together with the separate reconciliation path for externally originated exchange activity. It covers order intent, validation, compliance, conditional confirmation, submission, monitoring, lifecycle information, provenance, persistence, evidence, reporting, and analysis.

It is not an exact order-state machine or an exchange integration specification. Exchange identities, real orders and accounts, quantities, prices, fees, credentials, security configuration, exact rules and limits, provider protocols, database structures, and proprietary strategy logic are intentionally omitted. Lifecycle labels are durable capability descriptions and do not assert that every exchange exposes identical native states or transitions.

## Sources of trading activity

Adara distinguishes trading activity by provenance. That provenance preserves how an order entered the platform's operational view and determines what can be said about the controls applied before exchange submission.

### Discretionary trading

Discretionary trading begins with human-directed order intent created through Adara. An authorized user supplies the operational trading context, and the intended order proceeds through applicable validation and the platform's pre-trade compliance layer. After those controls succeed, a final human confirmation may be required before submission.

This path supports human decision-making without treating the interface action itself as sufficient authorization to reach an exchange. The controlled lifecycle includes the context and checks that precede submission as well as the state retained afterward.

### Automated strategies

Automated strategies operate inside the wider Adara platform and can create automated trading intent. Their orders use the same broader validation, compliance, order-management, and exchange-submission responsibilities as discretionary orders originated through Adara.

The shared control path does not mean that an automated order requires a human confirmation for every submission. Confirmation is a conditional interaction associated with discretionary execution where applicable. The public case study establishes that automated intent participates in the control and lifecycle framework without disclosing strategy names, indicators, parameters, formulas, or decision logic.

### Externally originated activity

An order may be created outside Adara at the exchange boundary. Adara can identify such activity when it is synchronized, retrieved, or observed and can retain it in the operational history with external provenance.

This is an observation and reconciliation path, not an Adara-controlled execution path. The fact that an externally originated order appears in Adara does not mean the platform authorized it, validated it, or applied pre-trade compliance before it reached the exchange. Retaining the activity makes the operational record broader while preserving the limit of what Adara controlled.

## Building an Adara-originated order

At a high level, an Adara-originated trading intent includes enough context to describe the requested exchange order. That context can include the relevant account, trading pair, buy or sell side, order type, quantity, and price where the order type requires one.

These are public order concepts rather than a complete internal data model or a reproduction of an input form. Adara can associate the intent with canonical asset and pair representations, current portfolio context, and its provenance as discretionary or automated activity. No real account, pair, quantity, price, or internal identifier is needed to explain the responsibility.

Building intent separately from submitting it matters because the platform can assess the request before it crosses the exchange boundary. Intent becomes an input to validation and compliance, not an instruction that bypasses them.

## Validation before execution

Before an Adara-originated order is submitted, the platform performs applicable validation. At public capability level, validation can consider order and input consistency, the available operational context, applicable exchange or trading constraints, and portfolio context where required.

The purpose is to determine whether the requested activity has the context needed to proceed into the controlled path. This description does not define provider-specific minimums, formulas, actual limits, or a fixed sequence of internal checks. It also does not claim that every order type or exchange capability requires an identical set of validation activities.

Validation and compliance are related but distinct responsibilities. Validation establishes applicable input and operating context; compliance evaluates the controls that govern whether the order can proceed toward submission.

## Pre-trade compliance gate

Orders originated through Adara are subject to applicable pre-trade compliance controls before exchange submission. This requirement applies to both discretionary orders and orders generated by automated strategies inside the platform.

Compliance may use normalized order value, account state, portfolio exposure, and portfolio-concentration context. If an applicable control fails, the order is stopped before the exchange boundary. The failed path is not depicted as reaching submission, and successful evaluation permits processing to continue through any applicable confirmation and then toward the exchange.

This placement makes compliance part of controlled execution rather than only a later reporting function. It does not imply that all activity visible at an exchange was controlled by Adara: externally originated orders are retained through the separate synchronization path described below.

The detailed public treatment of control categories, evidence, and auditability belongs in [Compliance and Audit](07-compliance-and-audit.md). Internal rule identifiers, thresholds, limits, formulas, and evaluation algorithms remain outside this lifecycle view.

## Confirmation where applicable

For human-directed discretionary activity, successful validation and compliance can be followed by a final confirmation before the order is submitted. This step keeps the confirmation associated with the human execution context and after the applicable controls have completed.

Confirmation is not presented as a universal state for every Adara-originated order. In particular, automated strategies are not described as requiring individual human confirmation for each order. The lifecycle diagram uses the explicit phrase “where applicable” to preserve that distinction.

## Exchange submission

After successful validation, successful compliance evaluation, and any applicable confirmation, Adara can submit an order directly to a digital-asset exchange. The platform can support exchange-specific capabilities while keeping the exchange boundary abstract in the public documentation.

Submission is the point at which the controlled Adara-originated path crosses to an external execution venue. The case study does not identify venues, endpoints, authentication methods, API-key handling, protocols, or security configuration.

## Monitoring and execution state

After submission, Adara manages and retains order lifecycle information. At a public capability level, that information can include submitted and open-order state, executed or closed state, cancelled state, filled quantity and execution information, execution-price information, fees, history, and provenance.

These terms describe information the platform can manage or retain; they do not define an exact finite-state machine or claim that all exchanges expose identical states and transitions. Monitoring connects external execution information with Adara's operational view so that the order can be reviewed beyond the initial submission.

Fill and fee information contribute to retained history and analysis, but no real fill, price, quantity, fee, position, or monetary value is published in this case study.

## Provenance and externally originated orders

Order provenance distinguishes at least three public classes: discretionary or human-directed activity, automated-strategy activity, and externally originated activity. The first two are Adara origins and follow the controlled validation and pre-trade compliance path. The third originates outside Adara.

Externally originated orders can enter the platform's history through synchronization, retrieval, or observation at the exchange boundary. Adara can reconcile that activity with its operational view and preserve its external provenance. It must not be inferred that these orders were pre-approved, validated, or compliance-checked by Adara before they reached the exchange.

This distinction separates **controlled execution** from **observed external activity**. Controlled execution documents the responsibilities Adara applies before submitting its own originated orders. Observed external activity documents what the platform can later see and retain. Combining both in a coherent history improves traceability without rewriting the origin or control history of an external order.

## Persistence, evidence and reporting

Adara retains operational state related to the order lifecycle and provenance. That state supports historical review, reporting, analytics, and the association of compliance evidence with activity originated through the platform. The public view does not prescribe a database schema, event model, messaging architecture, transaction boundary, or storage topology.

For an Adara-originated order that passes applicable controls and proceeds through the controlled execution path, the platform can generate order-level compliance evidence. An order-level compliance PDF documents the evaluation associated with that order, is distributed by email, and is retained server-side. The lifecycle view identifies this evidence boundary without exposing an actual report, recipient, email address, distribution list, or control value. It does not claim that a rejected pre-trade attempt generates the same public order-level PDF, while making no statement about other internal traces.

Externally originated activity can contribute to retained history and analytics, but its presence does not create evidence that Adara performed pre-trade controls before external submission. Provenance preserves this distinction when the combined operational history is reviewed.

## Relationship to analytics

Retained order state supports review of open orders, historical orders, executed trades and fills, fees, and provenance. It can also support pair and trading-performance analysis at a capability level.

Analytics depend on the meaning of the retained state. Origin, lifecycle information, execution details, and compliance evidence allow activity to be interpreted in context rather than as an undifferentiated set of exchange records. The public case study does not disclose real trading performance, fills, positions, fees, or financial values.

## Lifecycle diagram

The [Order Lifecycle Diagram](../diagrams/order-lifecycle.md) presents the two paths side by side. Three details are important:

1. Discretionary and automated activity originated through Adara converge on the controlled validation and pre-trade compliance path.
2. A failed applicable compliance result stops before exchange submission, while human confirmation is conditional rather than an automated-strategy requirement.
3. Externally originated activity joins persistent order history through synchronization or observation, not through a fictional Adara pre-trade gate.

The diagram is a responsibility and lifecycle view. It does not define internal implementation states or expose integration details.

## Public boundaries

This page does not identify exchanges, organisations, funds, stakeholders, accounts, orders, report recipients, or external providers. It contains no real identifiers, quantities, prices, fills, fees, positions, performance figures, or monetary values. Exact validation rules, compliance identifiers and limits, formulas, credentials, security settings, protocols, database structures, and proprietary strategy logic are also outside the public scope.

These boundaries keep the lifecycle focused on what the platform coordinates, where controls apply, how execution state is retained, and why provenance matters when controlled and externally originated activity coexist in one operational history.

[← Previous](05-market-data-and-portfolio.md) | [Case Study Home](../README.md) | [Next →](07-compliance-and-audit.md)

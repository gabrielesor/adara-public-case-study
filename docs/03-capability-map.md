# Capability Map

Adara contains more than 50 operational capabilities spanning the path from external market state to portfolio state, trading, controlled execution, retained order history, and operational reporting. This map groups those capabilities into eight public domains and shows how the domains cooperate as parts of one trading and investment-operations platform.

## Scope

This page is a capability map rather than an exhaustive feature inventory. It describes the responsibility of each domain, representative capabilities, and its relationships with the rest of the platform. Individual providers, exchanges, real accounts, orders, positions, financial values, exact compliance logic, and proprietary strategy decision logic are intentionally excluded.

The eight domains provide a stable product-level view of functional breadth. They do not reproduce an internal user-guide structure or disclose implementation details merely because a capability exists.

## Market Data

The Market Data domain brings changing external market state into Adara. It supports streaming and real-time ingestion from external public market-data providers and from digital-asset exchanges. Because external venues can use different identifiers for the same asset, the domain normalizes exchange-specific identifiers into a canonical internal model.

Representative capabilities include:

- streaming and real-time market-data ingestion;
- digital-asset exchange market data;
- canonical asset-identifier normalization;
- pair and asset market information; and
- historical market data at a product level.

Normalized market state can then be consumed consistently by portfolio valuation, discretionary trading, automated strategies, and analysis. The capability map does not identify individual providers or exchanges.

## Portfolio

The Portfolio domain maintains the account and portfolio state required for trading, controls, and reporting. Adara supports multiple accounts and models balances, valuations, composition, and exposure at both account and aggregate portfolio level. Heterogeneous values are normalized into a common reference currency so that controls and reports can operate on a consistent basis.

Representative capabilities include:

- multi-account portfolio state;
- balances and valuations;
- account-level and portfolio-level exposure;
- portfolio composition and historical portfolio information; and
- common reference-currency normalization.

Assets may be classified into three dynamically derived tiers based on market ranking and asset categories. Stablecoins and fiat currencies receive specific classification treatment. Exact algorithms, lists, conversion currencies, and real values are outside the public scope.

Portfolio state connects market data to the rest of the workflow: changing prices affect valuation, current exposure informs compliance evaluation, and execution results contribute to subsequent operational state.

## Trading

The Trading domain turns human-directed or automated intent into activity that can proceed through the platform's controlled execution path. It supports discretionary trading, automated trading, direct digital-asset exchange execution, validation, and capabilities specific to individual exchanges.

Representative capabilities include:

- discretionary, human-directed trading;
- automated trading;
- validation before submission;
- direct exchange execution; and
- support for exchange-specific capabilities.

Trading does not bypass the surrounding platform. Both discretionary and automated intent interact with portfolio state, order management, and the common pre-trade compliance layer. The public description establishes the supported modes without revealing how proprietary strategies generate decisions.

## Orders

The Orders domain manages an order as a lifecycle rather than a single submission request. It preserves the state and provenance required to distinguish discretionary, automated, and externally originated trading activity while supporting subsequent monitoring and analysis.

Representative capabilities include:

- order validation and submission;
- status monitoring and fill tracking;
- cancellation and fee handling;
- historical order analysis; and
- order provenance across discretionary, automated, and external origins.

Order state connects exchange execution to persistence, reporting, and analytics. This case study does not expose real order identifiers, account codes, references, quantities, or prices.

## Strategies

The Strategies domain supports the operational lifecycle of automated trading strategies. Adara provides capabilities for strategy configuration, execution, and monitoring, and algorithmic strategies may operate on real-time market state. The platform also supports automated grid-style execution.

Representative capabilities include:

- strategy configuration;
- execution and lifecycle management;
- operational monitoring;
- use of real-time market state; and
- automated grid-style execution support.

Strategies operate within the same broader platform as discretionary trading. Their orders remain connected to validation, compliance, order management, exchange execution, persistence, and reporting. Proprietary strategy names, indicators, parameters, formulas, decision logic, and detailed grid mechanics are not disclosed.

## Compliance

The Compliance domain places controls in the pre-trade path. Applicable controls are evaluated before an order is submitted to an exchange, and an order that fails those controls is prevented from reaching the exchange. The same compliance layer applies to discretionary and automated trading.

Representative capabilities include:

- trade-size controls;
- account-exposure controls;
- portfolio-exposure and portfolio-concentration controls;
- evaluation using values normalized into a common reference currency;
- multi-channel early-warning and compliance notifications; and
- order-level and daily portfolio-level compliance reporting.

Adara produces a compliance PDF for an individual order and a daily compliance PDF at portfolio level. These reports are distributed by email and retained server-side. Together with the retained order state, they provide evidence that compliance is part of execution rather than only a post-trade reporting activity.

The public capability map does not include internal rule identifiers, thresholds, limits, formulas, real reports, distribution lists, or exact schedule times. See [Compliance and Audit](07-compliance-and-audit.md) for the dedicated public treatment of this domain.

## Analysis & Reporting

The Analysis & Reporting domain turns retained operational state into views that support review and understanding of platform activity. Its scope includes portfolio analytics, exposure analysis, order history, trading analysis, profit-and-loss analysis at capability level, and compliance reporting.

Representative capabilities include:

- portfolio analytics and composition;
- account and portfolio exposure analysis;
- historical order analysis;
- pair and trading-performance analysis;
- profit-and-loss analysis; and
- compliance reporting.

This domain depends on normalized market and portfolio state, order provenance, execution records, and compliance evidence produced elsewhere in the platform. No real profit and loss, strategy performance, position, or financial exposure value is published.

## Administration & Operations

The Administration & Operations domain supports controlled use and continuing production operation of the platform. It covers administrative concepts and operational capabilities without exposing security configuration or operational secrets.

Representative capabilities include:

- users, roles, and access administration;
- platform settings;
- notification capabilities;
- scheduled processing; and
- external monitoring at a high level; and
- AI-assisted natural-language interaction through AiAlly.

These capabilities support the operating context around the functional workflow: access is administered, processing can be scheduled, notifications can be delivered, and the production web console can be observed through an external uptime-monitoring service. Credentials, API-key controls, IP restrictions, private endpoints, and other security-sensitive details remain outside the public scope.

AiAlly is a cross-cutting user and operational interface over product knowledge and a curated set of Adara capabilities exposed through MCP tools. Its OpenAI Assistants API generation is a retired historical production implementation. The replacement based on the OpenAI Responses API and remote MCP has been successfully validated in pre-production, and production rollout is pending. AiAlly is separate from automated strategy execution and proprietary trading decisions; see [AI-Assisted Operations — AiAlly](11-ai-assisted-operations.md).

## Cross-cutting workflow

The operational role of Adara comes from the integration of these domains rather than from isolated features. At a high level, they cooperate through the following sequence:

**Market state → portfolio state → human or automated trading intent → validation → compliance → exchange execution → persisted order state → reporting and analytics**

Market Data supplies normalized external state. Portfolio capabilities combine that state with account balances, valuations, composition, and exposure. A trader or automated strategy creates intent, which proceeds through validation and the same pre-trade compliance layer. If applicable controls pass, the order can be submitted through direct exchange integration. Orders then retain monitoring, fill, cancellation, fee, and provenance information for analysis and reporting.

Administration and operations support the workflow across its lifecycle through access administration, settings, notifications, scheduled processing, and external monitoring. The resulting platform connects decision inputs, controls, execution, persistent evidence, and review without publishing the proprietary mechanics of strategy decisions or confidential operational data.

[← Previous](02-role-and-history.md) | [Case Study Home](../README.md) | [Next →](04-system-architecture.md)

# Market Data and Portfolio

Adara has to reconcile two continuously changing realities: external market state and the balances, valuations, and exposure represented by its internal portfolio and account state. The platform turns heterogeneous market information and holdings into a coherent operational view that can support portfolio supervision, discretionary and automated trading, pre-trade compliance, reporting, and analytics.

This responsibility is broader than displaying prices or summing account balances. External systems may identify the same asset differently, accounts may serve different operational roles, and trading values may be expressed through heterogeneous pairs or currencies. Adara normalizes those inputs so other platform responsibilities can operate on shared asset, market, and portfolio concepts.

## Scope

This page describes the public product and architecture view of market-data integration, canonical asset normalization, the multi-account portfolio model, balances and portfolio state, valuation, exposure, asset classification, and historical state.

The focus is on why these responsibilities exist and how they connect. It does not reproduce a feature inventory or implementation guide. Provider-specific configuration, internal mapping tables, formulas, conversion paths, real holdings, valuations, exposures, account structures, and proprietary strategy logic are intentionally absent. The conceptual areas described here are not claims about specific classes, tables, modules, processes, or deployment components.

## External market state

Adara integrates with digital-asset exchanges and external public market-data providers. It receives real-time and streaming market information from those external boundaries and also supports historical market information at a capability level.

The market state relevant to the platform includes information associated with assets and trading pairs. At a public level, this covers current prices or quotations, pair and asset information, historical series and candle information, and market-ranking information. These capabilities provide inputs to valuation, trading context, asset classification, and analysis without making an external provider's representation the platform's internal model.

That distinction is important because each external source is an integration boundary, not the definition of an asset inside Adara. Exchange or provider data can be received and associated with a canonical asset representation before other responsibilities consume it. As a result, portfolio, trading, compliance, reporting, and analytical views can use consistent platform concepts rather than expose source-specific naming throughout their workflows.

This public view does not identify providers, exchanges, APIs, or protocols. It also does not describe feed priority, reconnection behavior, redundancy, internal threading, or latency characteristics.

## Canonical asset normalization

Different external systems may assign different identifiers to the same economic asset. If those external identifiers were allowed to propagate unchanged across portfolio and trading responsibilities, the platform would have to repeat provider-specific interpretation wherever market or holding information was used.

Adara addresses that problem by mapping exchange- and provider-specific identifiers to a canonical internal asset representation. The transformation can be summarized as:

**External asset identifiers → canonical internal asset representation**

The canonical representation provides a common reference for associating market information, trading pairs, balances, valuation state, orders, reports, and analytics. Portfolio state can therefore remain coherent across relevant integrations; trading and order logic can work with shared asset concepts; and reporting does not need to expose provider-specific naming as its organizing model.

Canonical normalization does not mean that exchange-specific capabilities disappear. Those capabilities can remain available at their integration boundary while the broader platform uses a consistent asset identity. The public description does not expose the mapping tables, lookup rules, configuration, or storage design used to achieve that result.

## Multi-account portfolio model

Adara maintains a multi-account portfolio model. The operational portfolio can be broader than one account at one exchange, and different accounts or holdings can serve different roles in the overall state.

At a conceptual level, those roles may distinguish:

- accounts enabled for trading operations;
- read-only accounts or holdings;
- externally managed accounts or holdings; and
- reference-data roles used to support market valuation or reference information.

These categories describe responsibilities within the portfolio model, not implementation types or a disclosure of the production account structure. They do not imply that every holding represented in the broader portfolio is controlled by Adara. In particular, read-only or externally managed holdings can contribute to portfolio context without being presented as accounts on which the platform performs trading operations.

Role-aware aggregation allows the platform to form a broader portfolio view from the accounts and holdings relevant to a given operational purpose. The account identities, providers, credentials, configuration, and fund-specific structure remain outside this case study.

## Balances and portfolio state

The portfolio model maintains balances and state at both account and portfolio level. Account-level state preserves the distinction between the relevant sources of holdings, while portfolio-level state can aggregate across applicable account roles to support a wider operational view.

That view includes balances, valuations, portfolio composition, account-level exposure, portfolio-level exposure, and historical portfolio information. The responsibilities are related but distinct: balances describe represented holdings; valuation places heterogeneous assets on a common reference basis; composition describes how the portfolio is distributed; and exposure provides control and analytical context at more than one level.

Maintaining the distinction between account and portfolio state matters when the same asset appears across several relevant holdings or when some holdings are informational rather than trading-enabled. Adara can preserve account-level context while also producing aggregate portfolio state for supervision, controls, and analysis.

No real balance, asset allocation, account identifier, portfolio value, or historical portfolio record is included in the public documentation.

## Valuation and reference normalization

Assets and trading activity can be expressed through heterogeneous pairs and currencies. Directly comparing those values would not provide a consistent basis for portfolio valuation, exposure, controls, or reporting.

Adara can normalize heterogeneous trading values into a common reference currency. At principle level, the transformation is:

**Heterogeneous asset and trading values → normalized reference value**

The normalized value provides a shared basis for portfolio valuation, account- and portfolio-level exposure, compliance evaluation, and reporting. It allows those responsibilities to compare values without treating the quote or currency used by each external pair as the final analytical unit.

This case study does not identify the reference currency, any intermediate conversion asset, conversion paths, price-selection rules, or mathematical formulas. It also makes no claim that a particular valuation configuration is universal; the public point is the responsibility for producing a common reference basis inside the platform.

## Exposure model

Adara models exposure at account level and at aggregate portfolio level. Account-level exposure preserves the context of an individual represented account or holding, while portfolio-level exposure provides a broader view across the relevant account roles included in the operational portfolio.

Exposure can also be considered alongside portfolio composition and asset classification. This supports analysis of how holdings are distributed and provides state that applicable compliance controls can consume before an order reaches an exchange. The compliance relationship is architectural: portfolio and order state provide inputs to evaluation, and failed applicable controls prevent submission.

The public model does not publish percentages, concentration thresholds, limits, formulas, or real exposure values. It documents the levels at which exposure is represented and the fact that the resulting state can support controls and analysis.

## Asset classification

Adara supports three dynamically derived asset tiers based on market-ranking information and asset categories. Stablecoins and fiat currencies receive specific classification treatment within this model.

The classification is an Adara portfolio and compliance concept, not a universal external financial-risk standard. Because it is dynamically derived, classification state may evolve when the underlying ranking or category information changes. This enables portfolio and compliance analysis to use current classification context without publishing source-specific labels throughout the platform.

The public case study deliberately omits tier boundaries, membership rules, ranking cutoffs, stablecoin lists, concrete rankings, and the detailed algorithm. It does not attach fixed risk meanings or real portfolio membership to any of the three tiers.

## Historical state

Adara supports historical views of market-related and portfolio state. Historical market information can include asset and pair series at a capability level, while historical portfolio information supports review of portfolio state and composition over time.

Scheduled processing may create or support historical snapshots. Those snapshots can provide retained reference points for historical review, composition analysis, and comparison across time without changing the distinction between current operational state and retained history.

The public description does not identify schedule times, internal jobs, job identifiers, recovery schedules, storage layout, or real historical values. It describes only the responsibility for retaining or generating historical views that other platform capabilities can use.

## Relationship to trading and compliance

Normalized market and portfolio state provide context to several other Adara domains. Authorized users can use current market, valuation, and exposure information when directing discretionary activity. Automated strategies may consume real-time market state. Trading and order management can use canonical asset identities and current portfolio context, while pre-trade compliance can evaluate applicable controls using normalized order and exposure values.

The same retained state supports reporting and analytics after operational activity has occurred. Detailed execution behavior belongs in [Order and Trading Lifecycle](06-order-and-trading-lifecycle.md), while the control model is treated separately in [Compliance and Audit](07-compliance-and-audit.md).

## Public boundaries

This page intentionally excludes provider and exchange identities, organisation and fund identities, production account names or codes, real balances, real valuations, real composition and exposure values, and real historical records. It also omits exact asset-classification rules, conversion currencies and formulas, provider configuration, credentials, private endpoints, access-control settings, and proprietary strategy decision logic.

Those boundaries keep the documentation focused on the architectural responsibilities: receiving heterogeneous external market information, normalizing asset identity and value, maintaining multi-account portfolio state, and supplying controlled inputs to trading, compliance, reporting, and analytics.

[← Previous](04-system-architecture.md) | [Case Study Home](../README.md) | [Next →](06-order-and-trading-lifecycle.md)

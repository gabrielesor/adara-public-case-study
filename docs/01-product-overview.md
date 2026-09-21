# Product Overview

Adara is a proprietary trading and investment-operations platform owned by Gabriele Soranzo. It coordinates market data, portfolio and account state, discretionary and automated trading, exchange execution, order management, pre-trade compliance, reporting, analytics, and administration within a single operational environment.

It has been used in production since June 2023 as internal operational tooling in a real investment-management environment. This context matters to the case study: Adara is not only an isolated trading algorithm, but a platform that supports the broader path from live market state and trading intent to controlled execution and retained operational evidence.

## Scope

This page introduces the operational problem addressed by Adara, the platform's role, its main capability domains, and its public technical positioning. It uses only information approved for public disclosure.

Detailed architecture, compliance controls, order flows, operational procedures, and scale are reserved for their dedicated case-study pages. Proprietary source code, real portfolio and transaction data, stakeholder identities, detailed deployment topology, exact compliance formulas and limits, and proprietary strategy decision logic are outside this overview.

## The operational problem

A trading operation must coordinate several concerns that change at different rates and originate in different systems. External market data changes continuously. Exchanges expose heterogeneous interfaces and exchange-specific asset identifiers and capabilities. Portfolio state must reflect balances, valuations, account exposure, and aggregate portfolio exposure. Human traders and automated strategies may both produce trading intent.

An order cannot be treated as a single submission action. Before submission, it requires validation and applicable compliance evaluation. After submission, the operation must monitor its status, track execution and fills, handle cancellation and fees, and preserve its provenance. The resulting state must remain available for historical analysis, reporting, and operational review.

These concerns are connected. Market state informs trading decisions and valuation. Orders affect account and portfolio state. Exposure values are inputs to compliance controls. Exchange responses affect the order lifecycle. Reporting depends on persistent, traceable operational data. A platform serving this workflow therefore needs to connect these responsibilities without reducing the operation to the decision logic of an individual strategy.

## Adara's role

Adara acts as the integration point for those operational concerns. It ingests real-time and streaming data from external public market-data providers and integrates directly with digital-asset exchanges. Exchange-specific asset identifiers are normalized into a canonical internal model, while exchange-specific capabilities remain available where required.

The platform maintains multi-account portfolio state, including balances and valuations, and models exposure at both account and portfolio level. Heterogeneous trading values are normalized into a common reference currency for controls and reporting. Assets may also be classified into three dynamically derived tiers based on market ranking and asset categories, with specific classification treatment for stablecoins and fiat currencies. The precise algorithms, lists, and conversion details are intentionally not public.

Adara connects that state to discretionary trading, automated strategy execution, order lifecycle management, compliance, reporting, analytics, and administrative operation. This makes it an operational platform rather than a stand-alone trading bot: strategy execution is one capability within a broader controlled workflow.

## Core capability domains

The public capability map groups more than 50 operational capabilities into eight domains:

### Market Data

Real-time and streaming ingestion brings external public market data into the platform. Normalization maps exchange-specific asset identifiers to a canonical internal model so that market state can be used consistently across other capabilities.

### Portfolio

The portfolio domain manages multiple accounts and their combined state. It covers balances, valuations, account-level exposure, portfolio-level exposure, and the normalization of heterogeneous values into a common reference currency for control and reporting purposes.

### Trading

Trading capabilities support both discretionary, human-directed activity and automated execution. Direct digital-asset exchange integrations provide the execution connection while allowing the platform to support capabilities specific to individual exchanges.

### Orders

Order management covers validation, submission, monitoring, execution and fill tracking, cancellation, fees, history, and provenance. The platform can distinguish discretionary, automated, and externally originated trading activity without exposing real identifiers, quantities, prices, or account information in this case study.

### Strategies

Adara hosts automated trading strategies and supports their configuration, execution, and monitoring. This includes support for automated grid-style execution, and algorithmic strategies may operate on real-time market state. The indicators, parameters, formulas, decision logic, and detailed mechanics of proprietary strategies are not part of the public case study.

### Compliance

Compliance controls are evaluated before an order is submitted to an exchange. Applicable categories include trade size, account exposure, portfolio exposure, and portfolio concentration. An order that fails applicable controls is prevented from reaching the exchange, and the same compliance layer applies to discretionary and automated activity.

Order values are normalized into a common reference currency for evaluation. Multi-channel early-warning and compliance-notification mechanisms support the process. Adara also produces an order-level compliance PDF for individual orders and a daily portfolio-level compliance PDF; reports are distributed by email and retained server-side.

### Analysis & Reporting

The platform supports historical order analysis, operational analytics, and reporting based on retained operational state. Compliance evidence includes both individual-order and daily portfolio-level reports.

### Administration & Operations

Administrative and operational capabilities support the continuing use of the platform in production. Detailed operating procedures, monitoring, recovery, and scheduled processing are addressed elsewhere in the case study.

## Discretionary and automated trading

Adara supports human-directed trading and automated strategies within the same broader operational platform. In both cases, trading intent proceeds through shared validation, compliance, order-management, execution-monitoring, persistence, and reporting concerns. This common operating context allows activity to be distinguished by provenance without creating a separate control model for automated orders.

The public scope demonstrates that Adara can host and operate algorithmic strategies, including grid-style execution that can act on real-time market state. It does not explain how proprietary strategies generate decisions.

## Compliance as part of execution

Compliance in Adara is part of the pre-trade order path rather than only a post-trade reporting activity. Applicable controls are evaluated before exchange submission. If an order fails those controls, it does not reach the exchange.

This placement matters operationally because the same enforcement layer applies whether the order originated from discretionary activity or an automated strategy. Normalized order values and portfolio state provide a common basis for evaluation, while retained reports provide order-level and daily portfolio-level evidence. The dedicated compliance page will examine this capability without publishing internal rule identifiers, thresholds, formulas, reports, or distribution details.

## Production use

Adara has operated in production since June 2023. It has processed more than 5,000 production orders and contains more than 50 operational capabilities. It has been used as internal operational tooling in a real investment-management environment.

The organisations, fund, and stakeholders involved are confidential and are not identified in this repository. Financial metrics and real operational records are not disclosed.

## Technical positioning

At the approved public level, Adara uses a Java backend, MySQL relational persistence, and AWS deployment. Its architecture is modular, maintains persistent operational state, and uses streaming connectivity for market-data processing. It integrates with external public market-data providers and directly with digital-asset exchanges.

This overview does not identify specific providers, exchanges, AWS services, component counts, or detailed topology. Those details are either deferred to later approved architecture material or remain outside the public scope.

## Public case-study boundaries

This repository contains documentation, not Adara's proprietary application source code. It excludes customer, fund, and stakeholder identities; real account identifiers; real orders, positions, balances, valuations, and transactions; exact compliance logic and limits; and proprietary strategy decision logic. Future visual material may use only synthetic or explicitly approved and sanitized data.

[← Previous](../README.md) | [Case Study Home](../README.md) | [Next →](02-role-and-history.md)

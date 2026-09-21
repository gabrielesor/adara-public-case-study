# System Architecture

Adara is a modular trading and investment-operations platform that connects streaming market state, multi-account portfolio state, discretionary and automated trading, pre-trade compliance, exchange execution, persistent evidence, reporting, operational administration, and AI-assisted interaction. Its public architecture is best understood as a set of cooperating logical responsibilities rather than as an inventory of physical components.

## Scope

This page presents a public logical architecture of Adara. It describes system boundaries, major responsibilities, and the principal information paths that connect them. The logical areas are architectural groupings used to explain the system; they do not necessarily correspond one-to-one to Java modules, JARs, processes, hosts, containers, or AWS resources.

This is not a deployment diagram, network topology, complete component inventory, JVM or process map, or source-code structure. Implementation-sensitive detail is deliberately omitted. The diagrams and prose focus on responsibilities that can be examined without exposing proprietary source code, security configuration, internal endpoints, or operational secrets.

## Architectural perspective

The central architectural problem is coordination across different forms of state and activity. External market state changes continuously and may arrive from digital-asset exchanges or public market-data providers. Asset identifiers specific to those sources must be normalized into a canonical internal representation before the resulting state can be used consistently by other platform responsibilities.

At the same time, Adara maintains balances, valuations, and exposure across multiple accounts. That portfolio state provides context for trading, compliance evaluation, and analytics. Trading intent may be created by an authorized operational user or by an automated strategy, but both origins participate in the wider validation, compliance, order-management, and execution path.

These concerns are separated as logical responsibilities while remaining integrated through operational state. Compliance uses portfolio and order context before exchange submission. Order management retains provenance and follows the lifecycle through execution and monitoring. Persistent state supports later reporting, analytics, administration, and retained compliance evidence. The architecture therefore connects decision inputs, controls, external execution, and review without treating any one strategy or exchange integration as the whole platform.

## System context

The [System Context Diagram](../diagrams/system-context.md) shows Adara as one system boundary surrounded by the external actors and systems with which it interacts.

Authorized operational users interact with the platform through its web-based operational console and can use AiAlly for natural-language interaction. Public market-data providers supply external market information. Digital-asset exchanges provide market and execution information and receive orders from Adara. The platform sends notifications and compliance reports to their intended recipients, while an external uptime-monitoring service observes the production web endpoint. The OpenAI API is an external service boundary for AiAlly interactions.

The context view deliberately keeps the identities of organisations, exchanges, data providers, recipients, and monitoring providers out of scope. OpenAI is named only to identify the authorized AiAlly service boundary. MySQL remains inside the Adara boundary because relational persistence is an internal platform responsibility rather than an external actor. No network layout, private endpoint, authorization method, or AWS service is implied by the context diagram.

## Logical architecture

The [Logical Architecture Diagram](../diagrams/logical-architecture.md) groups the platform into public logical responsibilities. These groupings explain how work is divided conceptually; they are not claims about runtime packaging or deployment units.

### Web Console

The Web Console is the web-based operational interface for authorized users. It provides access to operational interaction, discretionary trading, review, reporting, and administrative capabilities. The public architecture does not specify its frontend technology or its internal communication mechanisms.

### AiAlly / AI-Assisted Interaction

AiAlly provides a natural-language user and operational interface over product knowledge and selected Adara context or capabilities. The replacement architecture uses the OpenAI Responses API, remote MCP integration, optional File Search or RAG over product documentation, streaming responses, and operational tracing or diagnostics.

MCP represents a standardized boundary for a curated set of Adara capabilities exposed through MCP tools; it is not a claim that AiAlly owns the underlying product responsibilities or bypasses their workflows. The first-generation OpenAI Assistants API integration is a retired historical production implementation. The Responses/MCP replacement has been successfully validated in pre-production, and production rollout is pending. See [AI-Assisted Operations — AiAlly](11-ai-assisted-operations.md).

### Market Data & Normalization

Market Data & Normalization receives real-time and streaming market information from digital-asset exchanges and external public market-data providers. It normalizes exchange-specific asset identifiers into a canonical internal model. The resulting market state can support portfolio valuation, trading context, automated strategies, and analysis without requiring each responsibility to interpret external identifiers independently.

This view does not describe a redundancy design, source-specific protocol, or provider topology. It records only the responsibility for receiving and normalizing external market state.

### Portfolio & Valuation

Portfolio & Valuation maintains multi-account portfolio state, including balances, valuations, composition, and exposure at account and portfolio level. Heterogeneous trading values can be normalized into a common reference currency for controls and reporting.

Portfolio state is not isolated from the execution path. It contributes context to discretionary and automated trading, provides exposure and valuation information to compliance, and supports reporting and analytics. The architecture does not expose real positions, balances, valuation values, or conversion details.

### Trading & Order Management

Trading & Order Management accepts discretionary and automated trading intent and manages the order lifecycle. Its responsibility includes validation, submission, monitoring, execution and fill tracking, cancellation, fees, retained history, and provenance. Discretionary, automated, and externally originated activity can be distinguished in that retained state.

The public logical view shows the relationship between trading intent, compliance, exchange execution, and state retention. It does not describe internal order identifiers, account codes, implementation classes, threading, or endpoint structure.

### Strategy Lifecycle

Strategy Lifecycle represents configuration, execution, and monitoring of automated trading strategies as a platform responsibility. Strategies may consume real-time market state and create automated trading intent, but their orders use the same broader validation, compliance, order-management, and exchange-execution path as discretionary activity.

No proprietary strategy name, indicator, parameter, formula, decision rule, or detailed execution mechanic is part of this architecture view.

### Compliance

Compliance is positioned within the pre-trade execution path. It evaluates applicable controls using order and portfolio state before exchange submission. If an order fails applicable controls, it is prevented from reaching the exchange. The same compliance layer applies to discretionary and automated trading.

The responsibility also produces retained evidence and reporting. This logical view shows the placement and inputs of compliance without disclosing internal rule identifiers, thresholds, limits, formulas, report contents, or distribution details.

### Analysis & Reporting

Analysis & Reporting uses retained operational state to support review, historical analysis, portfolio and exposure analysis, trading analysis, and compliance reporting. Adara generates an order-level compliance PDF for individual orders and a daily portfolio-level compliance PDF. Those reports are retained server-side and distributed by email.

The architecture identifies these outputs without exposing actual reports, recipient addresses, distribution lists, real financial values, or the exact daily schedule.

### Administration & Notifications

Administration & Notifications groups user and role administration, settings, notification capabilities, and scheduled processing at a public level. It also provides the notification boundary through which operational and compliance information can be delivered.

Security remains intentionally abstract. The public view does not disclose credentials, access-control configuration, API-key handling, IP restrictions, private endpoints, or other security-sensitive operational details.

### Persistent Operational State

Persistent Operational State is the relational persistence responsibility used across the platform. Adara uses MySQL for relational persistence and maintains the operational state required by portfolio, order, compliance, administration, reporting, and analytical workflows.

This logical area does not imply a particular schema or database topology. Table design, replication, high-availability configuration, transaction-isolation choices, and caching are not described.

## Controlled execution path

At architecture level, the controlled path begins with normalized market state and current portfolio state. An authorized user or automated strategy produces trading intent. That intent proceeds through validation and applicable pre-trade compliance before an order can be submitted to a digital-asset exchange.

After submission, Trading & Order Management monitors execution and fills, retains provenance, and records lifecycle state. Persistent operational state then supports reporting, analytics, administration, and compliance evidence. This sequence is a responsibility-level view rather than a detailed order-state model; the dedicated [Order and Trading Lifecycle](06-order-and-trading-lifecycle.md) page provides the navigation point for that topic.

The placement of compliance is significant: discretionary and automated orders use the same broader control path, and failed applicable controls prevent exchange submission. The logical architecture therefore shows compliance between validated order state and the external execution boundary rather than treating it only as a reporting function.

## External integrations

Adara has two principal market-facing integration boundaries. Digital-asset exchanges provide market and execution information and receive submitted orders. External public market-data providers supply additional public market state. Exchange-specific identifiers are normalized before the resulting state is used across the platform, while exchange-specific capabilities remain available where required.

A notification and email boundary carries notifications and compliance reports to their recipients. Separately, an external uptime-monitoring boundary observes the production web endpoint. These are public responsibility boundaries only: no provider names, recipient addresses, protocols, private endpoints, or detailed integration topology are disclosed.

The OpenAI API provides the external AI-service boundary for AiAlly. The public architecture identifies Responses API interaction, remote MCP, optional File Search or RAG, and streaming at responsibility level without exposing endpoints, authorization, prompts, tool inventory, private documents, or configuration. This boundary describes the replacement validated in pre-production and does not present it as a current production deployment.

## Technology view

At a high level, Adara uses a Java backend, MySQL relational persistence, and deployment on AWS. The platform has a modular architecture, maintains persistent operational state, and uses streaming connectivity for external market information and exchange integration.

This technology view does not identify specific AWS services, module or JAR counts, process placement, host layout, or network structure. The named technologies establish the public technical context without turning the logical architecture into a deployment diagram.

## Deliberate public boundaries

The public architecture does not expose Adara's proprietary source code, Java package structure, module or JAR inventory, database schema, process or threading model, deployment topology, network topology, private endpoints, credentials, security configuration, proprietary strategy logic, private AiAlly prompts, tool configuration, identifiers, or source documents. It also avoids naming organisations, exchanges, market-data providers, monitoring providers, and report recipients.

Those omissions preserve the distinction between an inspectable technical case study and an implementation or operations manual. The material documents what the major responsibilities are, how they cooperate, and where external boundaries exist without publishing details that are unnecessary for understanding the architecture.

[← Previous](03-capability-map.md) | [Case Study Home](../README.md) | [Next →](05-market-data-and-portfolio.md)

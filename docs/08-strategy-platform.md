# Strategy Platform

Adara provides the operational infrastructure around automated and algorithmic trading strategies. A strategy can evaluate market information and create trading intent, but it does not operate as a detached script with a separate route to an exchange. It participates in the wider platform workflow for market state, validation, compliance, order management, monitoring, persistence, and analysis.

This page describes that hosting and execution environment. It focuses on how strategies are configured, operated, observed, and connected to controlled execution. The decision logic that determines when or why a proprietary strategy creates intent remains a deliberate public boundary.

## Scope

The public strategy-platform view covers lifecycle responsibilities, execution context, consumption of current or historical market state, generation of trading intent, use of the shared compliance and order path, management of multiple strategy instances, monitoring, retained activity, and analysis.

These responsibilities explain the engineering around automated execution without reproducing an algorithm. Exact signal logic, named indicators, formulas, periods, thresholds, parameters, account relationships, trading pairs, real instance records, positions, performance figures, and proprietary source code are outside the public scope.

The page presents durable capabilities rather than a complete internal model. It does not define a universal state machine, prescribe one lifecycle for every strategy category, or imply that every strategy consumes every item of market, portfolio, or account context available in Adara.

## Strategies as platform participants

Automated strategies operate inside the broader Adara platform. Their role is to turn evaluated information into trading intent while using platform responsibilities that also support other Adara-originated activity. This integration is the main architectural point: strategy execution is connected to normalized market state, trading and order management, applicable pre-trade compliance, persistent operational state, monitoring, and analysis.

The platform boundary allows a strategy to participate in a consistent operational workflow without making its decision logic part of every surrounding component. Market-data capabilities prepare current and historical information. Strategy lifecycle capabilities host and operate the configured automation. Trading and order management accept the resulting intent. Compliance determines whether the proposed order may proceed, and retained lifecycle state supports later monitoring and review.

Those relationships describe available platform context, not mandatory inputs for every algorithm. A particular strategy may use the subset of market and operational information relevant to its design. The public case study does not infer dependencies or decision factors that have not been disclosed.

## Strategy lifecycle

At a capability level, Adara supports the lifecycle needed to manage automated strategies over time. That lifecycle can include configuration, creation or initialization where applicable, activation, execution, monitoring, deactivation, and review of status or historical activity.

Configuration establishes the operational definition needed to manage an instance without exposing the configuration values themselves. Creation or initialization represents preparation of a runnable strategy where that concept applies. Activation makes an instance eligible to perform its configured work, while deactivation stops that ongoing participation. During execution, the platform can observe status and activity and connect resulting trading intent to the controlled order path. Historical review provides continuity beyond the currently active view.

These terms identify responsibilities rather than exact internal states or transitions. They do not assert that each strategy category exposes the same controls, progresses through an identical sequence, or uses the same execution cadence. The model is intentionally broad enough to describe lifecycle management without publishing implementation classes, scheduling mechanics, storage structures, or proprietary behavior.

## Market state and derived indicators

Automated strategies may consume current or historical market information available through Adara. Current state can support continuing evaluation as market conditions change, while retained information can provide historical context where a strategy design requires it.

Adara can also compute or use **derived indicators computed from market data**. Such values can contribute to algorithmic decision-making, but their presence does not reveal how a decision is formed. The public platform view establishes only that market information can be transformed into derived analytical inputs and made available within strategy execution.

No indicator is named here, and no calculation formula, period, threshold, combination, signal condition, or parameter is disclosed. The strategy's interpretation of market state remains inside the protected decision boundary. This separation demonstrates support for continuously evaluated market information while keeping trading logic out of the public case study.

## From strategy decision to controlled order

A strategy decision does not submit an exchange order directly. When a hosted strategy decides to act, it creates trading intent. That intent enters the same broader controlled execution path used by other orders originated through Adara:

**Strategy decision → trading intent → validation and context → applicable pre-trade compliance → exchange submission only if controls pass → monitoring and retained lifecycle state**

Validation establishes that the proposed activity has the applicable input and operating context. Compliance then evaluates the controls that apply before exchange submission. If any applicable control fails, the strategy-generated order does not reach the exchange. A passing result allows the controlled path to continue into order management and submission. Automated origin does not remove, bypass, or replace those responsibilities.

After submission, order-management responsibilities connect the activity to execution monitoring, lifecycle information, provenance, persistent operational state, and subsequent analysis. Provenance allows the platform to retain that the intent originated from an automated strategy while using the shared control and order infrastructure.

The detailed order path is documented in [Order and Trading Lifecycle](06-order-and-trading-lifecycle.md), and the control boundary is documented in [Compliance and Audit](07-compliance-and-audit.md). This page does not repeat their rule categories, evidence model, or order states; it shows where strategy execution joins those responsibilities.

## Multiple strategies and instances

Adara can manage multiple strategy instances. At public level, an instance represents an independently manageable occurrence of a strategy capability with its own configuration and trading context. Different instances can therefore coexist while remaining distinguishable for lifecycle operation and review.

The platform can provide instance-level status and activity views as well as aggregate and detail perspectives across strategy activity. An operator can move between a broader operational view and the context of an individual instance without treating all automated activity as one undifferentiated process.

Instance identity is described only as an operational concept. This case study publishes no real identifiers, counts, account associations, pairs, configuration values, parameters, positions, or performance results. It also does not claim that different strategy categories expose identical status information or analytical detail.

## Grid-style automation

Adara includes support for automated grid-style strategy execution and lifecycle management. It is one example of a strategy category hosted within the wider platform, using the surrounding responsibilities for controlled orders, monitoring, retained state, and analysis.

The category is sufficient for the public architecture view. Its construction, parameters, decision mechanics, order-generation behavior, capital allocation, and position-management behavior are intentionally not described.

## Monitoring and analysis

Strategy execution remains observable as part of platform operation. At a capability level, Adara can present strategy status, instance activity, aggregate and detail views, and the operational activity associated with automated execution. Retained strategy and order state can support historical review after immediate execution has completed.

Analysis capabilities can place strategy activity in the context of exposure and performance without publishing actual figures. The purpose is operational understanding: examining activity, distinguishing instances, relating intent to orders, and reviewing retained history. These capabilities do not establish strategy effectiveness, expected investment outcome, or comparative quality.

Monitoring and analysis also remain distinct from proprietary decision logic. Observing that an instance is active, reviewing its generated activity, or analyzing retained results does not disclose the rules that produced a decision. The platform can therefore support operational oversight while maintaining the intellectual-property boundary around signal generation and position management.

## Relationship to discretionary trading

Discretionary and automated trading share platform responsibilities including market and portfolio context, validation, pre-trade compliance, order management, exchange submission, execution monitoring, provenance, and retained state. Both create Adara-originated intent that enters the controlled path rather than bypassing it.

Their interaction models differ. A human-directed discretionary order can include an explicit confirmation where applicable. Automated activity proceeds according to its configured strategy lifecycle and is not described as requiring an individual human confirmation for every generated order. That distinction changes the interaction model, not the applicable compliance or order controls.

## Production operation

Automated strategy execution has been part of Adara's production operation. It runs within the monitored platform context described on this page rather than as a separate public proof of algorithm quality.

This statement does not attribute all platform orders to automation or make claims about uninterrupted execution, incident history, recovery from every failure, or investment results. Production operations and reliability are treated separately in [Operations and Reliability](09-operations-and-reliability.md).

## Proprietary strategy boundary

APCS documents the platform engineering required to host and operate algorithmic strategies within the wider Adara workflow. That includes lifecycle management, access to market state, creation of trading intent, integration with validation and compliance, order handling, monitoring, persistence, and analysis.

Proprietary strategy decision logic is intentionally outside the public scope. The case study does not publish proprietary strategy names, exact indicators, formulas, parameters, decision rules, signal generation, or position-management algorithms. These omissions preserve the distinction between explaining a production strategy platform and disclosing the intellectual property of the strategies it hosts.

The boundary is architectural rather than incidental. Readers can evaluate how automated intent is integrated with shared operational controls and retained state without needing the algorithm that selects a trade. Conversely, documenting the surrounding platform does not imply any conclusion about the quality or outcome of a protected strategy.

## Public boundaries

This page contains no real strategy instances, identifiers, account relationships, trading pairs, configurations, positions, exposures, returns, or performance values. It does not identify an organisation, stakeholder, exchange, market-data provider, private endpoint, credential, or sensitive operational setting.

Exact lifecycle states, internal schemas, scheduling details, failure procedures, strategy source code, signal logic, order-generation mechanics, and position-management methods are also omitted. The public record is limited to the responsibilities and relationships needed to understand Adara as an integrated strategy execution platform.

[← Previous](07-compliance-and-audit.md) | [Case Study Home](../README.md) | [Next →](09-operations-and-reliability.md)

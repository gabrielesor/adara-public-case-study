# Operational Scale

This page presents a deliberately small set of production-scale facts about Adara. The figures demonstrate real operational use and product breadth while keeping confidential financial, stakeholder, and implementation detail outside the public case study.

The evidence is aggregate by design. It is intended to help a reader understand the production history and operating context of the software, not to promote investment results or turn internal records into a public metrics catalogue.

## Production evidence at a glance

| Dimension | Public evidence |
| --- | --- |
| Production operation | Since June 2023 |
| Order processing | 5,000+ production orders processed |
| Product breadth | 50+ operational capabilities |

These are factual public summaries. The figures are intentionally presented as aggregate production metrics rather than exact internal counters, and no additional quantitative metric is needed to interpret them.

## Production duration

Adara has been in production since June 2023. This establishes that the software has supported real production operation over multiple years, rather than existing only as a prototype, demonstration, or test environment. It has been used as internal operational tooling in a real investment-management environment whose organisation, fund, and stakeholder identities are not disclosed.

Production duration describes elapsed operating history. It does not measure availability across that period, continuity of automated strategy execution, or the completion of every scheduled process. Those are different operational questions with different evidence boundaries. [Operations and Reliability](09-operations-and-reliability.md) explains why the externally monitored web endpoint, background execution, scheduled work, external dependencies, and persistent state must be considered separately.

The start date is therefore useful as evidence of sustained real-world use, but it is not converted into a platform-wide service claim.

## Order-processing scale

Adara has processed more than 5,000 production orders. The aggregate demonstrates repeated use of order-processing capabilities in production without publishing an exact internal count or dividing the activity by source.

Order processing sits inside a wider platform lifecycle. Adara coordinates order context, validation, compliance where applicable, exchange interaction, execution monitoring, retained state, provenance, reporting, and analysis. The number is evidence that this operational path has handled repeated production activity; it is not a statement about financial value, investment outcome, or the quality of an individual execution.

The figure is intentionally platform-level. It is not attributed solely to automated strategies, discretionary activity, or orders originated through Adara. The platform can also synchronize and retain externally originated exchange activity. Orders originated through Adara use the controlled validation and pre-trade compliance path described in [Order and Trading Lifecycle](06-order-and-trading-lifecycle.md), while externally originated activity retains its distinct provenance. The aggregate count does not erase that distinction or imply that every retained order passed through the Adara pre-trade gate.

## Capability breadth

Adara includes more than 50 operational capabilities. This figure describes the breadth of product responsibilities involved in operating the platform, not the size of one algorithm or a count of one implementation artifact type.

For readability, the public [Capability Map](03-capability-map.md) groups the broader product surface into eight domains:

- Market Data
- Portfolio
- Trading
- Orders
- Strategies
- Compliance
- Analysis & Reporting
- Administration & Operations

A capability is a product responsibility or function at the level used by this case study. It should not be interpreted one-to-one as a menu item, interface endpoint, source-code class, packaged library, deployable component, or database table. The domain grouping communicates what the product does and how its areas cooperate without reproducing an exhaustive internal inventory.

The capability figure complements the domain map: the aggregate communicates breadth, while the map provides the durable structure used to explain that breadth.

## What these figures establish

Together, the three public facts establish different aspects of Adara's production record:

- **Since June 2023** describes the duration of real production use.
- **5,000+ production orders processed** demonstrates repeated production order processing.
- **50+ operational capabilities** describes the breadth of platform responsibilities.

Their meanings should remain separate. Production duration is not an availability measure. Order count is not a financial or investment-performance figure. Capability count is not a deployment-component count. Taken together, they provide evidence of production use, repeated operational processing, and functional breadth without supporting conclusions about investment success, system capacity, regulatory status, or service availability.

## What is intentionally not quantified

The public evidence remains focused on software engineering and operational use. It does not quantify financial scale, fund size, account or portfolio holdings, individual order values, investment performance, real positions, infrastructure throughput, market-data volume, compliance-report volume, or availability measurements.

It also does not identify the organisation, fund, or stakeholders associated with the operating environment. These omissions preserve confidentiality and prevent unrelated financial or operational data from being mistaken for evidence about the architecture or product capabilities.

The three published aggregates are the complete quantitative production-scale disclosure in this case study.

## Relationship to the rest of the case study

Scale evidence supplements the technical narrative; it does not replace it. The [Capability Map](03-capability-map.md) explains the eight functional domains behind the breadth figure. [Order and Trading Lifecycle](06-order-and-trading-lifecycle.md) explains controlled execution, external provenance, and retained order state. [Compliance and Audit](07-compliance-and-audit.md) documents the pre-trade and evidence boundaries. [Operations and Reliability](09-operations-and-reliability.md) distinguishes monitoring and operational surfaces from scale.

Reading these pages together provides the intended interpretation: quantitative facts establish production context, while architecture and lifecycle documentation explain the responsibilities exercised within that context.

## Public boundaries

All figures on this page are aggregate public facts. Exact internal counts, confidential financial and stakeholder information, real orders and positions, detailed infrastructure measurements, and proprietary strategy logic remain outside the case-study scope.

This page provides a bounded record of production duration, repeated order processing, and product breadth. It introduces no additional metric or claim beyond those published facts.

[← Previous](09-operations-and-reliability.md) | [Case Study Home](../README.md) | [Next →](11-ai-assisted-operations.md)

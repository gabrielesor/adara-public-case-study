# Product Ownership and Engineering Role

This page is intentionally written in the first person because Adara is both a software product and a record of my own architecture and engineering work.

## What I owned

I conceived Adara, designed its original architecture, and initially implemented the platform end-to-end.

That work covered the system as an integrated whole:

- Java backend architecture and implementation;
- MySQL relational data model;
- direct exchange API and streaming/WebSocket integration;
- tick-level market-data ingestion and normalization;
- multi-account portfolio, valuation, and exposure model;
- discretionary and automated trading;
- strategy lifecycle and execution infrastructure;
- order creation, submission, synchronization, monitoring, fills, fees, and history;
- pre-trade compliance and retained compliance evidence;
- reporting, notifications, administration, and scheduled processing;
- AWS deployment and production operation; and
- later AI/MCP integration through AiAlly.

The important part is not that I touched many modules. It is that I owned the architectural boundaries between them: how market state becomes strategy/trader context, how intent becomes a controlled order, how portfolio state feeds compliance, how exchange execution is reconciled back into persistent history, and how the system remains explainable after the immediate trade has disappeared into history.

## From architecture to production

Adara was not handed off after a prototype. I took the original architecture through implementation and into production, where it has operated since June 2023.

The system has processed more than 5,000 production orders and more than €18M in aggregate traded volume through Adara-supported workflows. One retained nine-month market-data corpus contains more than 120 million tick-level observations.

These figures are included as engineering evidence: repeated production execution, sustained real-time data handling, and a system that has had to survive normal operational reality rather than only a controlled demonstration.

## Team evolution

A junior developer later contributed to selected development activities under my technical direction, especially user-interface work.

I retained architecture responsibility and the integration view of the product. The division of work is therefore best described as: **I designed and substantially built the platform; later development expanded through a junior contributor working under my technical direction.**

This case study does not minimize that contributor's work, but it also does not dilute ownership of the original architecture and the end-to-end system.

## Source provenance

The repository you are reading is a recent public case-study repository. It should not be mistaken for the age of the product.

The proprietary Adara source repository remains private. As of September 2026, it has a **five-year development history and 538 commits**. The private history is retained because the codebase contains proprietary strategy logic, security-sensitive configuration, and production implementation detail that should not be published merely to demonstrate authorship.

The public case study therefore exposes architecture, decisions, operating evidence, trade-offs, and lessons learned while keeping the application source private.

## AI authorship disclosure

Most of Adara's core platform predates generative-AI-assisted development.

The trading, market-data, portfolio, order-management, compliance, persistence, operational, and original strategy-hosting architecture was designed and implemented without generative AI being the development model behind the product.

Generative AI entered the project later in three bounded ways:

1. **AiAlly v2** — I used AI-assisted engineering while designing and implementing the OpenAI Responses API / remote MCP / document-retrieval generation.
2. **Junior development assistance** — the junior contributor used AI as a coding aid, particularly for portions of the UI.
3. **This public case study** — AI has been used to help structure, challenge, edit, and refine the documentation.

That distinction matters. Adara's core architecture is not a product generated retrospectively from an AI prompt; the AI layer and AI-assisted development came later.

## Architecture as an operating responsibility

I view the architecture of Adara as more than a component diagram. Three examples capture that responsibility:

- **Robustness by design:** external systems and connections can fail, so recovery and operational boundaries must be designed rather than hoped for.
- **Tick-driven by design:** if a strategy reacts to each price movement, the data path has to be built for that event model from the start.
- **Decision provenance by design:** if an automated order is questioned years later, the system should retain enough historical state to explain what happened and why.

The public case study is organized around those decisions because they are more representative of my work as an architect than a list of framework names.

## A real-system lesson: not every path aged equally

The tick-processing path was designed with strong latency awareness. The V1 order path accumulated synchronous work over time, including report generation and notification side effects, and observed end-to-end order creation could reach roughly 2–8 seconds.

I include that fact because architecture work also means recognizing where a successful production system has accumulated debt. Adara V2 is being designed with a much stricter separation between fast decision/execution paths and asynchronous audit, reporting, and notification work.

## Public boundary

This page documents engineering ownership, not investment-management responsibility. It does not identify the fund or confidential stakeholders and does not publish real account identifiers, positions, balances, credentials, proprietary strategy algorithms, or security-sensitive implementation detail.

[← Previous](01-product-overview.md) | [Case Study Home](../README.md) | [Next →](03-capability-map.md)

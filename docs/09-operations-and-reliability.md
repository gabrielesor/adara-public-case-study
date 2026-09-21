# Operations and Reliability

Operating Adara in production involves more than keeping a web page reachable. The platform combines an interactive web console, automated strategy execution, scheduled processing, external integrations, persistent operational state, notifications, and compliance-report delivery. Each has its own operational boundary and evidence.

Adara is designed and operated as a long-running production system, but this public case study does not compress those different responsibilities into one availability number. It reports scope-specific facts and explains what each fact can support without treating one monitored surface as proof for the complete platform.

## Scope

This page covers Adara's production operation since June 2023, external monitoring of the production web console, persistent operational state, scheduled processing, operational and compliance notifications, external dependencies, automated execution as a distinct operational surface, and the general handling of monitored issues.

The public view explains responsibilities and evidence boundaries rather than internal operating instructions. It excludes internal incident records, detailed logs, provider-specific events, private operating configuration, authentication and security details, deployment topology, unpublished service-level metrics, recovery internals, and the people or channels involved in operational support.

Reliability here means communicating what is monitored, retained, scheduled, or operationally handled at the appropriate scope. It is not a universal statement about every subsystem at every point in the platform's production history.

## Production operation

Adara has been in production since June 2023 and has been used as internal operational tooling in a real investment-management environment. Its operation includes both interactive activity through the web console and background responsibilities such as automated execution, data processing, state retention, and scheduled reporting.

This production history establishes that the platform has supported real operating workflows over time. Duration alone is not treated as evidence that every component, dependency, or scheduled activity was continuously available. The public account therefore separates production use from stronger reliability claims that the available evidence does not establish.

Adara uses a modular Java backend, MySQL relational persistence, and AWS deployment. These facts provide high-level technical context only. No individual infrastructure service, host, region, network arrangement, or deployment procedure is part of this operations view.

## Different reliability surfaces

Availability is not one-dimensional for a platform that combines interactive use, background execution, schedules, external systems, and retained state. A status observation about one surface must not automatically be interpreted as a platform-wide service commitment.

### Web console

The web console is the interactive surface through which authorized operational users work with Adara. Reachability of its production endpoint is observable from outside the platform and provides evidence about that endpoint at the time of observation.

### Automated strategy execution

Automated strategy execution is a background operational surface. It includes hosted strategy activity and the path from generated intent into shared validation, compliance, order management, and execution monitoring. Its operating status cannot be inferred solely from whether the web endpoint is reachable.

### Scheduled processing

Scheduled processing covers work initiated according to schedules rather than a direct interactive request. Historical state or snapshots, portfolio-related processing, and scheduled compliance reporting belong to this surface. A reachable console does not by itself establish the outcome of scheduled work.

### External integrations

Adara exchanges information with digital-asset exchanges, public market-data providers, email or notification boundaries, and cloud infrastructure. Their availability and behavior are part of the production environment but remain outside Adara's direct system boundary.

### Persistent operational state

Persistent state retains operational information beyond transient in-memory activity. It supports continuity of the platform view, order and compliance history, reporting, and later review. Persistence is a separate responsibility from endpoint reachability or the immediate execution of background work.

## External monitoring

The production web console is monitored through an external uptime-monitoring service. That service observes the configured production web endpoint from outside Adara, providing an independent operational signal about endpoint availability and making endpoint-level interruptions observable.

The scope of that evidence is deliberately narrow. External observation of the web console does not prove the availability of automated strategies, scheduled jobs, exchange connectivity, market-data delivery, report generation, or every internal component. This case study does not extrapolate endpoint observations into a lifetime availability percentage for the complete Adara platform.

The monitoring provider, its configuration, measurement intervals, notification routing, and historical measurements remain outside public scope. The documented fact is the existence of external web-endpoint monitoring, not a broader certification of platform operation.

## Persistent operational state

Adara maintains persistent operational state through MySQL relational persistence. Retained information supports order lifecycle state, order provenance, historical activity, portfolio-related history, compliance evidence, reporting, and operational review.

Persistence matters because the platform's operational view is not limited to values held temporarily during one process or interaction. Retained state connects current operation with prior activity and allows histories and evidence to remain available for later review. It also supports continuity of the product view across interactive, automated, and scheduled responsibilities.

This public description identifies the persistence responsibility and named technology only. It does not define physical topology, data-placement design, backup design, isolation behavior, or recovery objectives. Those details are not necessary to understand why persistent state is part of the operational model.

## Scheduled processing

Adara includes scheduled processing for operational activities such as historical state or snapshots, portfolio-related processing, and scheduled compliance reporting. These activities execute according to operating schedules and contribute to retained information and reporting without requiring a user to initiate each run interactively.

Adara supports scheduled daily portfolio-level compliance reporting. The resulting PDF represents portfolio-level compliance status, is distributed by email, and is retained server-side. This is a supported operational capability; it is not a claim that every scheduled run throughout production history completed without exception.

Scheduled work is therefore evaluated as its own operational surface. Web-endpoint reachability, strategy status, and schedule execution answer different questions. Exact execution times, job names, scheduling implementation, and internal execution behavior are not published.

## Notifications and retained evidence

Operational and compliance events can produce notifications, including multi-channel notifications where applicable. Notifications provide an operational signal to support awareness and review, while retained state and evidence provide information that remains available after the immediate event.

For successfully controlled Adara-originated orders, order-level compliance PDFs can be generated, distributed by email, and retained server-side. Scheduled portfolio-level compliance evidence is a separate view. Their detailed control and evidence boundaries are described in [Compliance and Audit](07-compliance-and-audit.md).

This page does not identify recipients, addresses, routing configuration, or delivery guarantees. Notification and evidence capabilities contribute to operations, but neither is treated as a universal measure of system availability.

## External dependencies

A production trading platform operates through external boundaries as well as its own components. Adara depends operationally on digital-asset exchanges for market and execution interaction, public market-data providers for external information, email and notification boundaries for delivery, and cloud infrastructure for deployment.

External systems can fail, delay responses, or become unavailable. Their behavior can affect the platform's operating environment even when an Adara-owned surface remains reachable. Conversely, successful observation of the web console says nothing definitive about the contemporaneous status of each dependency.

The public case study records these dependencies as engineering boundaries. It does not identify providers, describe provider-specific events, or assert undocumented fallback, retry, or recovery behavior. Detailed integration and support procedures remain private.

## Automated execution in production

Automated strategy execution has formed part of Adara's production use. It operates within the same wider platform that supplies market state, validation, applicable pre-trade compliance, order management, monitoring, and retained lifecycle state.

This fact establishes production use, not an availability measure for strategy execution. No strategy-operation percentage, complete execution-history claim, restart behavior, or incident assertion is made. The engineering lifecycle around hosted automation is described in [Strategy Platform](08-strategy-platform.md).

## AI-assisted operations status

AiAlly's first-generation OpenAI Assistants API integration is a historical production implementation that is currently unavailable following retirement of the upstream API. Its historical production status does not imply that it remains a working user capability.

The replacement based on the OpenAI Responses API, remote MCP, and optional document retrieval has been successfully validated in pre-production. Production rollout is pending. Its status must therefore remain separate from both the legacy production history and the production status of the wider Adara platform. See [AI-Assisted Operations — AiAlly](11-ai-assisted-operations.md).

## Operational handling

Production issues can require detection, investigation, validation of the affected scope, and controlled restoration of service. At public abstraction level, operational issues are handled through monitored investigation and controlled restoration procedures.

The sequence describes operating discipline rather than a fixed workflow for every condition. It does not specify a recovery time, claim complete automation, or publish internal troubleshooting steps, support assignments, private channels, or a historical event. The appropriate action depends on the affected surface and its operational context.

Scope matters during handling. An endpoint observation, external dependency problem, delayed scheduled activity, or automated-execution issue can require different investigation even though all belong to the same production platform. The public model preserves those distinctions without presenting an internal runbook.

## Reliability claims and evidence boundary

Reliable technical communication connects each statement to the evidence it actually describes. In Adara's public case study, external monitoring applies to the production web endpoint being observed. Scheduled processing concerns independently initiated background work. Automated strategy operation concerns hosted automation and its controlled execution path. Persistent state concerns retained information and review. External integrations introduce dependencies whose status is not established by the console monitor.

These surfaces interact, but they are not interchangeable measurements. A positive observation for one cannot be generalized into a lifetime availability result for all the others. Accordingly, no single global uptime percentage is asserted for the complete Adara platform.

This boundary is not a limitation of the architecture description; it is a precision requirement. It allows the case study to demonstrate real production operation, external endpoint monitoring, scheduled capabilities, retained state, and operational procedures without converting partial evidence into a broader promise.

## Relationship to operational scale

Reliability evidence and scale evidence answer different questions. This page explains operational surfaces, monitoring scope, state retention, dependencies, and handling boundaries. [Operational Scale](10-operational-scale.md) separately addresses the published quantitative evidence about production duration, order processing, and capability breadth.

Keeping those topics separate prevents production volume or feature count from being interpreted as an availability measure. This page does not duplicate that quantitative scale treatment.

## Public boundaries

This operations view omits internal incident records, detailed logs, monitoring-provider identity and configuration, exact schedules, private notification routes, security configuration, deployment hosts and topology, provider-specific events, service-level measurements, and internal restoration procedures.

It also contains no private account or portfolio data, financial values, strategy results, or confidential stakeholder identities. Public documentation remains at responsibility level: what operational surfaces exist, what evidence is retained or observed, and where reliability conclusions must remain scoped.

[← Previous](08-strategy-platform.md) | [Case Study Home](../README.md) | [Next →](10-operational-scale.md)

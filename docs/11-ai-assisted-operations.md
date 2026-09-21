# AI-Assisted Operations — AiAlly

AiAlly is Adara's AI-assisted user interface. It provides a natural-language interaction layer for product knowledge and selected Adara operational information or capabilities while preserving the boundaries of the wider platform.

AiAlly has evolved through two technical generations. The first is a historical production implementation that is currently unavailable following retirement of the upstream OpenAI Assistants API. Its replacement has been designed and implemented with the OpenAI Responses API and remote Model Context Protocol (MCP) integration and has been successfully validated in pre-production; production rollout is pending. Keeping those statuses separate is essential to the public architecture.

## Scope

This page describes AiAlly as an interaction responsibility: what it is, how natural-language requests can combine product knowledge with selected operational context, how the two generations differ, and which engineering capabilities support the replacement implementation.

The public view covers the OpenAI API boundary, a curated set of Adara capabilities exposed through MCP tools, optional File Search or retrieval-augmented generation (RAG) over Adara product documentation, streamed responses, application context, conversation continuity where configured, and operational observability.

It does not document private prompts or instructions, tool inventories, connection details, authorization, user identities, source-document locations, configuration values, or production data. It also does not extend AiAlly into automated trading or proprietary strategy decision-making.

## AiAlly as a user interface

AiAlly allows a user to interact with Adara through natural language. The interaction can address product knowledge and, where exposed through the controlled integration boundary, operational Adara context or capabilities. The objective is to make relevant platform information accessible through a conversational interface without redefining the underlying responsibilities that own that information.

This distinction matters architecturally. AiAlly is an interface over selected knowledge and capabilities, not a replacement for portfolio, order, compliance, reporting, administration, or strategy-lifecycle responsibilities. Those areas remain part of Adara's established architecture. AiAlly coordinates an assisted interaction with them through public integration concepts rather than becoming the source of their operational rules.

Natural-language interaction also does not imply unrestricted platform access. The public case study establishes that the replacement uses a curated MCP tool boundary, but it does not publish the inventory, permissions, or configuration behind that boundary.

## First generation: production Assistants integration

The first generation of AiAlly was based on the OpenAI Assistants API. It was deployed and used as part of the production Adara environment, establishing a historical production implementation of AI-assisted interaction within the product.

That generation is currently unavailable as a working user capability following retirement of the upstream Assistants API. This status preserves its production history without implying that it still functions normally or that its implementation has necessarily been physically removed.

No adoption level or production-usage count is disclosed. The relevant fact is architectural and historical: Adara integrated an AI-assisted interface in production, and the upstream lifecycle of its API later required a replacement.

## Re-architecture

The replacement is more than an endpoint substitution. It reorganizes the interaction around the OpenAI Responses API, a remote MCP boundary, selected Adara tools and capabilities, optional product-document retrieval, streaming, application context, conversation continuity where configured, and operational tracing and diagnostics.

At public architecture level, these concerns cooperate as follows:

- the Responses API provides the model interaction and tool-orchestration boundary;
- remote MCP provides a standardized boundary through which that orchestration reaches selected Adara capabilities and receives their results;
- File Search or RAG can supply relevant product knowledge from Adara documentation;
- streaming returns response content progressively to the user interface;
- page or application context can inform the interaction;
- conversation continuity can preserve prior interaction context where configured; and
- tracing, tool/source visibility, and usage diagnostics support operational review.

The replacement has been designed and implemented and has been successfully validated in pre-production. Production rollout remains pending. References to its architecture describe the replacement design and validated pre-production behavior, not a currently deployed production integration.

## MCP-based operational context

The replacement uses remote MCP integration as the standardized tool boundary between the Responses interaction and selected Adara capabilities. Publicly, this is described as **a curated set of Adara capabilities exposed through MCP tools**.

AiAlly sends the interaction through the OpenAI Responses API. When tool use is relevant, the Responses orchestration invokes the remote MCP boundary, receives selected capability results through that interaction, and can use them in the model response streamed back through AiAlly. This keeps AiAlly conceptually separate from the platform areas that own portfolio, order, compliance, reporting, or administrative behavior.

The public case study does not enumerate tools, assign individual permission semantics, or expose connection and authorization configuration. It makes no claim about a specific tool being observational or state-changing; those details are outside the public architecture and cannot be inferred from the existence of MCP integration.

## Product knowledge through retrieval

AiAlly can combine MCP-provided operational context with product knowledge retrieved from Adara documentation through optional OpenAI File Search or RAG. Retrieval gives the interaction a way to consult relevant documentation in addition to context obtained through the standardized tool boundary.

These are complementary sources. MCP connects the interaction to selected product capabilities or operational context, while document retrieval supplies knowledge expressed in product documentation. Tool and source observability can help distinguish where supporting context originated without exposing the private source material itself.

No vector-store identifier, file identifier, document path, private document, or retrieval configuration is published. The case study describes the responsibility and source boundary, not the contents or physical arrangement of the retrieval corpus.

## Interaction flow

The replacement interaction can be summarized at responsibility level:

**User → AiAlly → OpenAI Responses API → model reasoning and tool selection → remote MCP and/or document retrieval → capability or retrieval results return through the Responses interaction → response streamed through AiAlly to the user**

AiAlly supplies the interaction boundary and applicable page or application context. The Responses API supports the model exchange. When relevant, the Responses interaction can invoke MCP tools for selected Adara context, consult product documentation through retrieval, or combine both source types before response content is streamed back.

This is a conceptual flow, not a network sequence, protocol specification, or claim that every request uses every source. A request may depend on product knowledge, operational context, or neither, according to the interaction and configured capabilities.

## Operational engineering

The replacement includes engineering concerns needed to operate and review an assisted interface. Streaming responses allow the interface to present output progressively. Page and application context can ground a request in the user's current product interaction. Conversation continuity can be applied where configured rather than assumed for every exchange.

Request and response tracing supports investigation of an interaction path. Tool and source visibility helps identify whether operational context or retrieved documentation contributed to a response. Usage diagnostics provide operational information about the integration without turning private request content, identifiers, or configuration into public documentation.

These capabilities support observability of the assisted interaction. They do not establish correctness guarantees, unrestricted access, or a bypass around existing platform authorization and workflow boundaries.

## Status

The two implementation statuses are distinct:

| Generation | Public status |
| --- | --- |
| OpenAI Assistants API integration | Historical production implementation; currently unavailable following retirement of the upstream API |
| OpenAI Responses API with remote MCP integration | Designed and implemented; successfully validated in pre-production; production rollout pending |

The historical production statement applies only to the first generation. It must not be transferred to the Responses/MCP replacement. Conversely, the currently unavailable first generation must not be presented as a working production capability.

## Separation from algorithmic trading

AiAlly is an AI-assisted user and operational interface. It is separate from Adara's automated trading strategies and from the proprietary decision logic described in [Strategy Platform](08-strategy-platform.md).

AiAlly is not presented as a trading strategy, a trading decision engine, or a system that chooses buy or sell actions. No public claim is made that AiAlly can place orders. Its integration does not imply a bypass of authorization, compliance, trading controls, or existing platform workflows.

This boundary allows the case study to document natural-language access to product knowledge and selected operational context without conflating assisted interaction with automated execution.

## Public boundaries

This page excludes private prompts and instructions, credentials, API or MCP endpoints, authorization methods, allowed-tool configuration, tool inventories, user identifiers, conversation identifiers, trace identifiers, vector-store and file identifiers, private document locations, and private production data.

It also excludes proprietary source code, proprietary trading logic, real accounts, orders, positions, balances, and confidential organisation or stakeholder information. The public architecture is limited to the interaction responsibilities, technology boundaries, observability concepts, and implementation status needed to understand AiAlly without exposing its private operating configuration.

[← Previous](10-operational-scale.md) | [Case Study Home](../README.md)

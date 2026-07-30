# Bitrix24 Cloud and Self-Hosted

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 operates as both a cloud service and as self-hosted installations on a client's server. This section helps prepare a REST application to work with the self-hosted version: accounting for the differences of the self-hosted version, opening the necessary network access on the client side, and complying with security requirements.

If you are just starting, open [REST API Peculiarities in the Self-Hosted Version](on-premise/index.md) — it covers the main differences between the self-hosted version and the cloud. Network access and security will be required during the implementation stage at the client site.

## All Section Materials

#|
|| **Material** | **Description** ||
|| [REST API features in the boxed version](on-premise/index.md) | How the boxed version differs when working with methods, events, and authorization. Module versioning, isolated box, and adding your own methods ||
|| [Required network access](network-access.md) | Which inbound and outbound requests to open for the application to work with the boxed Bitrix24 ||
|| [Security recommendations for REST API applications](security-recommendations.md) | Working with tokens, data validation, and communication channel protection ||
|#

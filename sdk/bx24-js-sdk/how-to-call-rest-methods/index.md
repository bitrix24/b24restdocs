# Calling REST Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Calling a **REST** method is a cross-domain request to the current Bitrix24 REST service on behalf of the current user. The authorization of the request is performed automatically based on the OAuth 2.0 protocol.

- [Sending a request](./bx24-call-method.md)
- [Processing the request result](./bx24-call-method.md#ajax-result)
- [Batch execution of requests](./bx24-call-batch.md)
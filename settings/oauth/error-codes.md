# Error Codes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

#|
|| **Code** | **Description** ||
|| `invalid_request` | An incorrectly formatted authorization request was provided ||
|| `invalid_client` | Invalid client data was provided. The application may not be installed in Bitrix24 ||
|| `insufficient_scope` or `invalid_scope` | Access permissions requested exceed those specified in the application card ||
|| `invalid_grant` | Invalid authorization tokens were provided when obtaining `access_token`. This occurs during renewal or initial acquisition ||
|#
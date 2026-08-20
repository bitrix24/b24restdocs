# Authorization Server Error Codes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The authorization server `oauth.bitrix.info` returns the error code in the `error` field and the explanation in the `error_description` field. The error arrives instead of a pair of tokens — either when [exchanging the authorization code](index.md#app-authorization) or when [renewing the tokens](auto-renewal.md).

```json
{
    "error": "PAYMENT_REQUIRED",
    "error_description": "Payment required"
}
```

#|
|| **Code** | **Description** | **What to Do** ||
|| `invalid_request` | An incorrectly formatted authorization request was provided | Check the composition and spelling of the request parameters ||
|| `invalid_client` | Invalid client data was provided. The application may not be installed in Bitrix24 | Check `client_id` and `client_secret`, and make sure that the application is installed in this Bitrix24 ||
|| `insufficient_scope` or `invalid_scope` | Access permissions requested exceed those specified in the application card | Add the missing permissions to the application card ||
|| `invalid_grant` | Invalid authorization data was provided for obtaining `access_token`. This occurs both when tokens are obtained for the first time and when they are renewed | If `refresh_token` has expired, go through the authorization again using the [complete protocol](index.md) ||
|| `PAYMENT_REQUIRED` | The trial or paid period of the application has expired | Renew the application payment. Until then, the authorization server will not issue tokens ||
|#

The errors that Bitrix24 itself returns on method calls — for example, `expired_token` for an expired `access_token` — are collected in the general list [Error Codes](../../error-codes.md).
# WebRTC

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 allows you to embed an external WebRTC client into the web version of the product. This scenario has no placement code of its own: the client is loaded into the `PAGE_BACKGROUND_WORKER` placement, and telephony is connected with the methods of the [{#T}](../../telephony/index.md) section.

The scenario has no visible element in the interface either. The user works with the standard call card, while the application's WebRTC client runs in the background.

To embed your own WebRTC client:

1. Upload your WebRTC client to a [dedicated placement](../universal/background-worker.md) `PAGE_BACKGROUND_WORKER`.
2. When an incoming call arrives, register it using the standard telephony integration method [{#T}](../../telephony/telephony-external-call-register.md), which also displays the standard call card to the user.
3. Manage the state and buttons of the call card using [special js methods](../ui-interaction/page-background-worker/index.md) available for the widget handler `PAGE_BACKGROUND_WORKER`.
4. After the call ends, notify Bitrix24 about it using the method [{#T}](../../telephony/telephony-external-call-finish.md).

The rest — for example, uploading call recordings to Bitrix24 — is implemented with the [telephony integration methods](../../telephony/index.md), without referring to the WebRTC client.

If your application needs its own tab in the call card, use the [{#T}](./call-card.md) placement.

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./call-card.md)
- [{#T}](../universal/background-worker.md)
- [{#T}](../ui-interaction/page-background-worker/index.md)
- [{#T}](../placement-bind.md)
- [{#T}](../../telephony/index.md)
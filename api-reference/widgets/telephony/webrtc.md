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

When registering the `PAGE_BACKGROUND_WORKER` handler, the `OPTIONS[errorHandlerUrl]` parameter is required: if the handler took longer than five seconds to load more than ten times a day, Bitrix24 deletes the registration and reports it with a request to the specified address.

{% include [Note on Examples](../../../_includes/examples.md) %}

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "PLACEMENT": "PAGE_BACKGROUND_WORKER",
    "HANDLER": "https://your-domain.com/widgets/webrtc-worker.php",
    "TITLE": "WebRTC client",
    "OPTIONS": {
      "errorHandlerUrl": "https://your-domain.com/widgets/webrtc-error.php"
    },
    "auth": "**put_access_token_here**"
  }' \
  https://**put_your_bitrix24_address**/rest/placement.bind
```

The workflow with the call card is described in the article [{#T}](../ui-interaction/page-background-worker/webrtc-scenario.md), and the full composition of the handler data is on the placement page [{#T}](../universal/background-worker.md).

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| `placement.bind` returns `EMPTY_ERROR_HANDLER_URL` | The `PAGE_BACKGROUND_WORKER` placement requires an address for disconnection messages. Pass `OPTIONS[errorHandlerUrl]` ||
|| `placement.bind` returns `ERROR_PLACEMENT_MAX_COUNT` | A handler for this placement is already registered: it is registered in a single instance. Remove the old registration with the [placement.unbind](../placement-unbind.md) method ||
|| `placement.bind` returns `WRONG_AUTH_TYPE` with the description `Application context required` | Register the placement on behalf of an application. A placement cannot be bound with a webhook ||
|| The background handler is no longer called | The registration was removed because of slow responses. Bitrix24 reports it to the address from `OPTIONS[errorHandlerUrl]`; after fixing the issue, register the handler again ||
|#

Other registration error codes are listed in the "Possible Error Codes" section of the [placement.bind](../placement-bind.md) page.

The rest — for example, uploading call recordings to Bitrix24 — is implemented with the [telephony integration methods](../../telephony/index.md), without referring to the WebRTC client.

If your application needs its own tab in the call card, use the [{#T}](./call-card.md) placement.

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./call-card.md)
- [{#T}](../universal/background-worker.md)
- [{#T}](../ui-interaction/page-background-worker/index.md)
- [{#T}](../ui-interaction/page-background-worker/webrtc-scenario.md)
- [{#T}](../placement-bind.md)
- [{#T}](../../telephony/index.md)
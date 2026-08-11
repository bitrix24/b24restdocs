# WebRTC Embedding Scenario

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A telephony application with its own WebRTC client works with the Bitrix24 call card in two steps: it registers a background handler on all pages and raises the card with a call of its own. The commands and events of the card are listed in the section overview [{#T}](./index.md).

## Handler Registration

The handler of the WebRTC client is registered once and loads on every page.

There is a dedicated placement for this: it is formed as an invisible frame on every Bitrix24 page. The registration is done as follows:

```php
'placement.bind',
[
    'PLACEMENT' => 'PAGE_BACKGROUND_WORKER',
    'HANDLER' => 'http://example.com/placement/?ty=1',
    'OPTIONS' => [
        'errorHandlerUrl' => 'http://example.com/logg.php?ty=1',
    ],
    'LANG_ALL' => [
        'de' => [
            'TITLE' => 'test',
        ]
    ]
]
```

An important distinction from other placements is the mandatory `OPTIONS[errorHandlerUrl]` parameter. The widget loads on every page, so the handler has to respond quickly: if the response takes longer than five seconds and this happens more than ten times a day on the same Bitrix24, the registration is deleted. Bitrix24 reports the deletion with a request to the address from `errorHandlerUrl`, after which the handler has to be registered again. Read more in the article [{#T}](../../universal/background-worker.md).

## Usage Scenario

The call is registered with the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method — the same method raises the call card. Call it at the moment when the WebRTC client of the widget starts processing the call.

Furthermore, the widget can interact with the open call card. For working with the card through the `PAGE_BACKGROUND_WORKER` placement, there are 9 JS interface commands for retrieving and modifying card data and 17 events for handling operator actions — the full list is in the section overview [{#T}](./index.md).

The key event is [BackgroundCallCard::initialized](./events/initialized.md). It occurs when the call card is created, and only after that can the card be managed. Therefore, make all command calls from the application side in the handler of this event.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./card.md)
- [{#T}](./events/index.md)
- [{#T}](../../universal/background-worker.md)
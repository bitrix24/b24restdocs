# WebRTC Embedding Scenario

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

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

The work with telephony remains the same as it was. The call registration is performed using the method [telephony.externalcall.register](../../../telephony/index.md). This same method "raises" the call detail form. It is evident that this should occur if the WebRTC client in this widget has started processing the call.

Furthermore, the widget can interact with the open call detail form, managing buttons and button press events. For working with the call detail form through the placement **PAGE_BACKGROUND_WORKER**, 9 methods have been added to retrieve and modify detail form data and 17 events for handling user activities.

The key event is `BackgroundCallCard::initialized`. It is triggered upon the creation of the call detail form, and after that, it becomes possible to manage this detail form. Therefore, it is strongly recommended that all method calls from the application side be made specifically in the event handler function for this event.
# WebRTC Integration Scenario

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

## Integration Registration

Registration of the integration for the "client" on each page.

There is a special integration area that is formed as an invisible frame on each Bitrix24 page. The registration is done as follows:

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

An important distinction from a standard integration is the `Options[errorHandlerUrl]` parameter. This handler receives a signal that we are disabling the integration on a specific Bitrix24 account if the handler specified in the Handler responds too slowly. Since the integration is formed on each page, it is crucial that the integration handler is called quickly (currently, we consider "not quickly" to be when the handler is called for more than 5 seconds three times). In case of a shutdown, the handler will need to be re-registered in this Bitrix24.

## Usage Scenario

The work with telephony remains the same as it was. The call registration is performed using the method [telephony.externalcall.register](../../../telephony/index.md). This same method "raises" the call detail form. It is evident that this should occur if the WebRTC client in the integration described above has started processing the call.

Furthermore, the integration can interact with the open call detail form, managing buttons and button press events. For working with the call detail form through the placement **PAGE_BACKGROUND_WORKER**, 9 methods have been added to retrieve and modify detail form data and 17 events for handling user activities.

The key event is `BackgroundCallCard::initialized`. It is triggered upon the creation of the call detail form, and after that, it becomes possible to manage this detail form. Therefore, it is strongly recommended that all method calls from the application side be made specifically in the event handler function for this event.
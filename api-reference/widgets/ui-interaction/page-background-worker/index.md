# WebRTC Integration Scenario

## Integration Registration

Registration of the integration for the "client" on each page.

There is a special integration area that is formed as an invisible frame on each Bitrix24 page. Registration is done as follows:

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

An important distinction from a regular integration is the `Options[errorHandlerUrl]` parameter. This handler receives a signal that we are disabling the integration on a specific Bitrix24 account if the handler specified in Handler responds too slowly. Since the integration is formed on each page, it is crucial that the integration handler is called quickly (currently, we consider "not quickly" to be when the handler takes longer than 5 seconds three times). In case of a shutdown, the handler will need to be re-registered in this Bitrix24.

## Usage Scenario

Working with telephony remains the same as before. Call registration is performed using the method [telephony.externalcall.register](../../../telephony/index.md). This same method "raises" the call card. Obviously, this should occur if the WebRTC client in the integration described above has started processing the call.

Furthermore, the integration can interact with the open call card, managing buttons and button press events. For working with the call card through the **PAGE_BACKGROUND_WORKER** placement, 9 methods have been added to retrieve and modify card data, along with 17 events to handle user activity.

The key event is `BackgroundCallCard::initialized`. It is triggered upon the creation of the call card, after which it becomes possible to manage this card. Therefore, it is strongly recommended that all method calls from the application side be made specifically in the event handler function for this event.
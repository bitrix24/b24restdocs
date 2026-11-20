# WebRTC Embedding Scenario

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A telephony application with its own WebRTC client works with the Bitrix24 call card in two steps: it registers a background handler once and then registers each call, after which Bitrix24 shows the call card. The commands and events of the card are listed in the section overview [{#T}](./index.md).

## Handler Registration

The handler of the WebRTC client is registered once and loads on every page.

There is a dedicated placement for this — an invisible frame on every Bitrix24 page. The application registers a handler in it with the [placement.bind](../../placement-bind.md) method from its own interface, for example during installation. The application needs the `placement` and `telephony` scopes. Who can call `placement.bind` is described on the method page. The handler starts loading once the application installation is complete.

The examples use [BX24.js](../../../../sdk/bx24-js-sdk/index.md) — the library is included on the application page. A registration example:

```js
BX24.callMethod(
    'placement.bind',
    {
        PLACEMENT: 'PAGE_BACKGROUND_WORKER',
        HANDLER: 'https://your-domain.com/widgets/call-worker.html',
        TITLE: 'WebRTC client',
        OPTIONS: {
            errorHandlerUrl: 'https://your-domain.com/widgets/call-worker-error.php'
        }
    },
    function (result) {
        if (result.error()) {
            console.error(result.error());
        } else {
            console.log(result.data()); // true
        }
    }
);
```

The same call in other languages is shown in the article [{#T}](../../universal/background-worker.md).

An important distinction from other placements is the mandatory `OPTIONS[errorHandlerUrl]` parameter. The widget loads on every page, so the handler has to respond quickly: if the handler frame takes longer than five seconds to load and this happens more than ten times a day on the same Bitrix24, the registration is deleted. Bitrix24 reports the deletion with a request to the address from `errorHandlerUrl`, after which the handler has to be registered again. Read more in the article [{#T}](../../universal/background-worker.md).

## Usage Scenario

The call is registered with the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method. The same method shows the call card to the employee specified in the `USER_ID` parameter. Call the method when the WebRTC client of the widget starts processing the call. The background handler subscribes to the card events earlier — on load, with the [BX24.placement.bindEvent](../bx24-placement-bind-event.md) method. The handler does not receive an event that arrived before the subscription.

A call example:

```js
BX24.callMethod(
    'telephony.externalCall.register',
    {
        USER_ID: 1,                  // the operator to whom Bitrix24 shows the card
        PHONE_NUMBER: '+49301234567', // the other party's number
        TYPE: 2,                     // 2 — incoming call
        SHOW: 1                      // show the card immediately
    },
    function (result) {
        if (result.error()) {
            console.error(result.error());
        } else {
            console.log(result.data().CALL_ID); // call ID for telephony.externalCall.finish
        }
    }
);
```

Furthermore, the widget can interact with the open call card. For working with the card through the `PAGE_BACKGROUND_WORKER` placement, there are 9 JS interface commands for retrieving and modifying card data and 16 events for handling operator actions — the full list is in the section overview [{#T}](./index.md).

The key event is [BackgroundCallCard::initialized](./events/initialized.md). It occurs when the call card is created, and only after that can the card be managed. Therefore, call the commands after it arrives. The exception is the [CallCardGetListUiStates](./call-card-get-list-ui-states.md) command: it does not need a card. The other party's number and the CRM link arrive before `initialized` — in the [BackgroundCallCard::entityChanged](./events/entity-changed.md) event, in the `PHONE_NUMBER`, `CRM_ENTITY_TYPE`, and `CRM_ENTITY_ID` fields. This event arrives only if the card loads CRM data.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./card.md)
- [{#T}](./events/index.md)
- [{#T}](../../universal/background-worker.md)
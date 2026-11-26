# Call Card CALL_CARD: Overview of Commands and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `CALL_CARD` placement adds an application widget as a tab in the CRM call card. Through the JS interface, the application retrieves data of the current call, controls the auto-closing of the card, and reacts to a change of the client or of the call state.

{% note warning "" %}

The commands and events of this section are not REST methods. The application calls them in the browser through the JS interface of the placement: commands with the [BX24.placement.call](../bx24-placement-call.md) method, event subscriptions with the [BX24.placement.bindEvent](../bx24-placement-bind-event.md) method. There are no requests to `/rest/` with the names `getStatus` or `CallCard::EntityChanged`.

{% endnote %}

> Quick navigation: [all commands and events](#all-methods)
>
> User documentation: [Call Card](https://helpdesk.bitrix24.com/open/18493064/)

## Getting Started with CALL_CARD

1. Register the `CALL_CARD` placement with the [placement.bind](../../placement-bind.md) method — the widget appears as a tab in the call card. How the placement itself is arranged and which data the handler receives when it opens is described on the page [{#T}](../../telephony/call-card.md)
2. Make sure the widget is open in this exact card: the list of available commands and events is returned by [BX24.placement.getInterface](../bx24-placement-get-interface.md)
3. Retrieve the data of the current call with the [getStatus](./get-status.md) command
4. Subscribe to changes in the card with the [BX24.placement.bindEvent](../bx24-placement-bind-event.md) method

A minimal working cycle: the application reads the call data and tracks the change of its state.

```js
BX24.ready(function () {
    BX24.init(function () {
        BX24.placement.call('getStatus', {}, function (status) {
            console.log(status.CALL_ID, status.PHONE_NUMBER);
        });

        BX24.placement.bindEvent('CallCard::CallStateChanged', function (callState) {
            console.log(callState);
        });
    });
});
```

## How a Placement Command Call Works

A command is invoked by name: pass the command name as the first argument of [BX24.placement.call](../bx24-placement-call.md), the parameters object as the second, and the callback function as the third.

None of the `CALL_CARD` commands accepts parameters — pass an empty object `{}` as the second argument.

The `getStatus` command returns the data of the current call, while the auto-close commands return an empty array. They have no error codes of their own.

If the widget is open outside the call card, the placement interface ignores the unknown command: the callback function is not invoked at all.

This is what the `getStatus` response looks like for an incoming call from a contact who has a deal:

```json
{
    "CALL_ID": "externalCall.440f80efd51b08c719713286819ab5d7.1795683625",
    "PHONE_NUMBER": "+491701234567",
    "LINE_NUMBER": "493012345678",
    "LINE_NAME": "Sales Department",
    "CRM_ENTITY_TYPE": "CONTACT",
    "CRM_ENTITY_ID": 4597,
    "CRM_BINDINGS": [
        {
            "ENTITY_TYPE": "DEAL",
            "ENTITY_ID": 9221
        },
        {
            "ENTITY_TYPE": "CONTACT",
            "ENTITY_ID": 4597
        }
    ],
    "CALL_DIRECTION": "incoming",
    "CALL_STATE": "connected",
    "CALL_LIST_MODE": false
}
```

The response fields are described on the [getStatus](./get-status.md) command page.

Events arrive in the callback function passed to [BX24.placement.bindEvent](../bx24-placement-bind-event.md). The handler receives client data or the call state, and when the card is closing, it is called without data.

## How Auto-Closing of the Card Works

By default, the card disappears right after the conversation, and the widget closes with it. To give the user time to fill in the application form, auto-closing is blocked.

1. The conversation ends, and Bitrix24 tries to close the card
2. If auto-closing is blocked by the [disableAutoClose](./disable-auto-close.md) command, the [CallCard::BeforeClose](./call-card-before-close.md) event arrives and the card stays on screen for another 65 seconds. Each repeated call of `disableAutoClose` restarts the countdown. A comment form open in the card also delays closing by 65 seconds
3. When the application has finished its work, it calls [enableAutoClose](./enable-auto-close.md). The block is lifted. If the 65-second countdown is already running and the user has no other active call, the card closes immediately. If the conversation is still in progress, the card closes after it ends
4. If the application has not called `enableAutoClose`, Bitrix24 closes the card by itself after 65 seconds, provided that the user has no other active call. Before closing, Bitrix24 retains the open comment

```js
BX24.ready(function () {
    BX24.init(function () {
        // the user started filling in the form — do not let the card close
        BX24.placement.call('disableAutoClose', {}, function () {
            console.log('auto-closing is blocked');
        });

        // the event arrives twice: on the attempt to close the card and on closing
        var saving = false;

        BX24.placement.bindEvent('CallCard::BeforeClose', function () {
            if (saving) {
                return;
            }
            saving = true;

            // saveFormData is an application function: it saves the form and returns a Promise
            saveFormData().then(function () {
                // no callback function: the card closes immediately along with the widget
                BX24.placement.call('enableAutoClose', {});
            }, function () {
                // not saved: reset the flag and extend the delay — the card stays open for another 65 seconds
                saving = false;
                BX24.placement.call('disableAutoClose', {});
            });
        });
    });
});
```

## How to Choose a Command or an Event

#|
|| **Scenario** | **What to use** ||
|| You need the data of the current call: identifier, number, line, CRM binding | The [getStatus](./get-status.md) command ||
|| The widget shows a form, and the card must not close by itself after the call ends | The [disableAutoClose](./disable-auto-close.md) command, and once the data is saved — [enableAutoClose](./enable-auto-close.md) ||
|| You need to refresh the widget interface when the card loads or refreshes the client data: on opening, after a lead is created, when moving on to the next client in a call campaign | The [CallCard::EntityChanged](./call-card-entity-changed.md) event. It may arrive again for the same client ||
|| You need to save data before the card disappears | The [CallCard::BeforeClose](./call-card-before-close.md) event together with the [disableAutoClose](./disable-auto-close.md) command ||
|| The widget has to react to the connection and the end of the conversation | The [CallCard::CallStateChanged](./call-card-call-state-changed.md) event ||
|#

## Overview of Commands and Events {#all-methods}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — access to the call card placement
>
> Who can execute the commands: any user

{% list tabs %}

- Commands

    #|
    || **Command** | **Description** ||
    || [getStatus](./get-status.md) | Retrieves information about the current call ||
    || [disableAutoClose](./disable-auto-close.md) | Disables automatic closing of the card after the call ends ||
    || [enableAutoClose](./enable-auto-close.md) | Enables automatic closing of the card after the call ends ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [CallCard::EntityChanged](./call-card-entity-changed.md) | When the client data is loaded or refreshed, including a repeated load for the same client ||
    || [CallCard::BeforeClose](./call-card-before-close.md) | When an attempt is made to close the call card, including an unsuccessful one ||
    || [CallCard::CallStateChanged](./call-card-call-state-changed.md) | When the state of the current call changes ||
    |#

{% endlist %}

## Typical Errors

#|
|| **Problem** | **Cause and what to do** ||
|| The callback function is not invoked | The widget is open outside the `CALL_CARD` placement, or the command name is misspelled. Check the list of available commands with the [BX24.placement.getInterface](../bx24-placement-get-interface.md) method. If the function is not invoked after `enableAutoClose`, this is not an error: the command closed the card along with the widget ||
|| The card closes while the user is filling in the widget form | Auto-closing is not disabled, or the 65-second delay has expired. Call the [disableAutoClose](./disable-auto-close.md) command, and if you need more time, call it again — the countdown restarts. When you are done, restore auto-closing with the [enableAutoClose](./enable-auto-close.md) command ||
|| The call data contains an empty `CRM_ENTITY_TYPE` or `CRM_ENTITY_ID = 0` | The call is not bound to a CRM object. Handle this case separately instead of treating it as an error ||
|| An attempt to control the card buttons, change the title, or change the interface state | There are no such commands in `CALL_CARD`. These are capabilities of the background placement — see [{#T}](../page-background-worker/index.md) ||
|#

## Continue Learning

- [{#T}](../../telephony/call-card.md)
- [{#T}](../bx24-placement-call.md)
- [{#T}](../bx24-placement-bind-event.md)
- [{#T}](../page-background-worker/index.md)
- [{#T}](../index.md)

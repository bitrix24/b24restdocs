# Call Card CALL_CARD: Overview of Commands and Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

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

Events arrive in the callback function passed to [BX24.placement.bindEvent](../bx24-placement-bind-event.md). The handler receives client data, the call state with additional parameters, or a call without data if the card is closing.

## How Auto-Closing of the Card Works

By default, the card disappears right after the conversation, and the widget closes with it. To give the user time to fill in the application form, auto-closing is blocked.

1. The conversation ends, and Bitrix24 tries to close the card
2. If auto-closing is blocked by the [disableAutoClose](./disable-auto-close.md) command, the [CallCard::BeforeClose](./call-card-before-close.md) event arrives and the card stays on screen for another 65 seconds. Each repeated call of `disableAutoClose` extends the countdown
3. When the application has finished its work, it calls [enableAutoClose](./enable-auto-close.md). The block is lifted, and the card closes if the conversation has already ended

```js
BX24.ready(function () {
    BX24.init(function () {
        // the user started filling in the form — do not let the card close
        BX24.placement.call('disableAutoClose', {}, function () {
            console.log('auto-closing is blocked');
        });

        BX24.placement.bindEvent('CallCard::BeforeClose', function () {
            // the card is about to be closed: save the form data
            saveFormData().then(function () {
                BX24.placement.call('enableAutoClose', {}, function () {
                    console.log('auto-closing is restored');
                });
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
|| You need to refresh the widget interface when the client bound to the call changes: the card pulled up CRM data, the operator created a lead or moved on to the next client in a call campaign | The [CallCard::EntityChanged](./call-card-entity-changed.md) event ||
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
    || [CallCard::EntityChanged](./call-card-entity-changed.md) | When the client bound to the call changes ||
    || [CallCard::BeforeClose](./call-card-before-close.md) | When an attempt is made to close the call card, including an unsuccessful one ||
    || [CallCard::CallStateChanged](./call-card-call-state-changed.md) | When the state of the current call changes ||
    |#

{% endlist %}

## Typical Errors

#|
|| **Problem** | **Cause and what to do** ||
|| The callback function is not invoked | The widget is open outside the `CALL_CARD` placement, or the command name is misspelled. Check the list of available commands with the [BX24.placement.getInterface](../bx24-placement-get-interface.md) method ||
|| The card closes while the user is filling in the widget form | Auto-closing is not disabled. Call [disableAutoClose](./disable-auto-close.md), and do not forget to restore auto-closing with the [enableAutoClose](./enable-auto-close.md) command ||
|| The call data contains an empty `CRM_ENTITY_TYPE` or `CRM_ENTITY_ID = 0` | The call is not bound to a CRM object. Handle this case separately instead of treating it as an error ||
|| An attempt to control the card buttons, change the title, or change the interface state | There are no such commands in `CALL_CARD`. These are capabilities of the background placement — see [{#T}](../page-background-worker/index.md) ||
|#

## Continue Learning

- [{#T}](../../telephony/call-card.md)
- [{#T}](../bx24-placement-call.md)
- [{#T}](../bx24-placement-bind-event.md)
- [{#T}](../page-background-worker/index.md)
- [{#T}](../index.md)

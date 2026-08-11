# Overview of Events for Working with the Call Card of a WebRTC Client

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The `BackgroundCallCard::*` events let an application react to the operator's actions in the call card and to changes of the interface state without additional polling.

{% note warning "" %}

These are events of the placement JS interface, not [REST events](../../../../events/index.md). They have no server handler: the application subscribes to them in the browser with the [BX24.placement.bindEvent](../../bx24-placement-bind-event.md) method. They cannot be registered with the `event.bind` method, and the REST method `placement.bindEvent` does not exist.

{% endnote %}

> Quick navigation: [all events](#all-events)

## How to Subscribe to Events

1. Register the `PAGE_BACKGROUND_WORKER` placement with the [placement.bind](../../../placement-bind.md) method — the procedure is described in the section overview [{#T}](../index.md)
2. In the widget code, subscribe to the events you need with the [BX24.placement.bindEvent](../../bx24-placement-bind-event.md) method
3. Raise the call card with the [telephony.externalCall.register](../../../../telephony/telephony-external-call-register.md) method. Events arrive only for an external call made by the application: during a regular Bitrix24 call, the card looks the same, but none of the `BackgroundCallCard::*` events fire
4. Wait for the [BackgroundCallCard::initialized](./initialized.md) event, and only after it manage the card with [BX24.placement.call](../../bx24-placement-call.md) commands

```js
BX24.ready(function () {
    BX24.init(function () {
        BX24.placement.bindEvent('BackgroundCallCard::initialized', function (eventData) {
            console.log(eventData.CALL_ID);
        });

        BX24.placement.bindEvent('BackgroundCallCard::muteButtonClick', function (eventData) {
            // eventData = true if the operator muted the microphone
        });
    });
});
```

You need to subscribe to each event separately: there is no subscription to all events at once.

An interface event cannot be unsubscribed from, and calling `BX24.placement.bindEvent` again with the same name adds a second handler, so the application handles the event twice. Subscribe once per widget load.

## How to Choose an Event

#|
|| **Scenario** | **What to use** | **What arrives in the handler** ||
|| The application gained access to the created call card | [BackgroundCallCard::initialized](initialized.md) | The initial call data and the CRM bindings ||
|| The operator clicks the call control buttons | [BackgroundCallCard::muteButtonClick](mute-button-click.md), [BackgroundCallCard::holdButtonClick](hold-button-click.md), [BackgroundCallCard::hangupButtonClick](hang-up-button-click.md), [BackgroundCallCard::answerButtonClick](answer-button-click.md) | The data of the specific action in the interface ||
|| The operator works with the call transfer | [BackgroundCallCard::transferButtonClick](transfer-button-click.md), [BackgroundCallCard::cancelTransferButtonClick](cancel-transfer-button-click.md), [BackgroundCallCard::completeTransferButtonClick](complete-transfer-button-click.md) | The data of the transfer scenario ||
|| The card loaded the client data from the CRM, the client was identified, or the current client changed in a call campaign | [BackgroundCallCard::entityChanged](entity-changed.md) | The client's number and the current CRM binding ||
|| You need additional actions from the card interface, for example to save a comment, rate the call quality, or enter a digit on the keypad | [BackgroundCallCard::addCommentButtonClick](add-comment-button-click.md), [BackgroundCallCard::dialpadButtonClick](dialpad-button-click.md), [BackgroundCallCard::qualityMeterClick](quality-meter-click.md), [BackgroundCallCard::notifyAdminButtonClick](notify-admin-button-click.md), [BackgroundCallCard::nextButtonClick](next-button-click.md), [BackgroundCallCard::skipButtonClick](skip-button-click.md), [BackgroundCallCard::makeCallButtonClick](make-call-button-click.md), [BackgroundCallCard::closeButtonClick](close-button-click.md) | The value selected by the user or the parameters of the action ||
|#

Which buttons the operator sees in each state of the card and which event they trigger is shown on the page [{#T}](../card.md).

## Overview of Events {#all-events}

> Scope: [`placement`](../../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** | **Data in the handler** ||
|| [BackgroundCallCard::initialized](initialized.md) | After the call card is created | An object with the call data ||
|| [BackgroundCallCard::addCommentButtonClick](add-comment-button-click.md) | When a comment is saved in the call card | A string with the comment text ||
|| [BackgroundCallCard::muteButtonClick](mute-button-click.md) | When the mute button is clicked | `boolean` — the state of the microphone ||
|| [BackgroundCallCard::holdButtonClick](hold-button-click.md) | When the hold call button is clicked | `boolean` — the hold state ||
|| [BackgroundCallCard::closeButtonClick](close-button-click.md) | When the close call card button is clicked | No data ||
|| [BackgroundCallCard::transferButtonClick](transfer-button-click.md) | When the current operator selects the operator to transfer the call to | An object with the number and the target of the transfer ||
|| [BackgroundCallCard::cancelTransferButtonClick](cancel-transfer-button-click.md) | When the "return to call" button is clicked | No data ||
|| [BackgroundCallCard::completeTransferButtonClick](complete-transfer-button-click.md) | When the "redirect" button is clicked | No data ||
|| [BackgroundCallCard::hangupButtonClick](hang-up-button-click.md) | When the "end" button is clicked | No data ||
|| [BackgroundCallCard::nextButtonClick](next-button-click.md) | When the "next" button is clicked | No data ||
|| [BackgroundCallCard::skipButtonClick](skip-button-click.md) | When the "skip" button is clicked | No data ||
|| [BackgroundCallCard::answerButtonClick](answer-button-click.md) | When the "answer" button is clicked | No data ||
|| [BackgroundCallCard::entityChanged](entity-changed.md) | When the CRM object linked to the call is loaded or changed | An object with the number and the CRM binding ||
|| [BackgroundCallCard::makeCallButtonClick](make-call-button-click.md) | When the "call" or "callback" button is clicked | No data ||
|| [BackgroundCallCard::qualityMeterClick](quality-meter-click.md) | When the call quality is rated | A string with a rating from 1 to 5 ||
|| [BackgroundCallCard::dialpadButtonClick](dialpad-button-click.md) | When one of the numeric buttons of the phone is pressed | A string with the pressed key ||
|| [BackgroundCallCard::notifyAdminButtonClick](notify-admin-button-click.md) | When the "notify administrator" button is clicked | No data ||
|#

## Typical Errors

#|
|| **Problem** | **Cause and what to do** ||
|| The handler is never invoked | The widget is open outside the `PAGE_BACKGROUND_WORKER` placement. In other placements, the `BackgroundCallCard::*` events are not registered, and the subscription silently fails ||
|| The widget is in the right placement, but events do not arrive | The call is not an external one. Events are emitted only for a card raised with the [telephony.externalCall.register](../../../../telephony/telephony-external-call-register.md) method; for calls made by Bitrix24 itself, the interface stays silent ||
|| The handler is not invoked for one event only | The event name is misspelled or uses different capitalization. The list of events of the current placement is returned by [BX24.placement.getInterface](../../bx24-placement-get-interface.md) ||
|| Events arrive, but the card management commands return an error | The commands were invoked before the [BackgroundCallCard::initialized](initialized.md) event, when there was no card yet ||
|| The application waits for a confirmation that the event was handled | The events are one-way: a handler cannot return a value to Bitrix24, and the application reacts with a separate command call or REST method call ||
|| The application changed the card with a command and waits for a corresponding event | The `CallCardSetMute` and `CallCardSetHold` commands change the card directly, and the `muteButtonClick` and `holdButtonClick` events do not occur: they arrive only in response to the operator's actions ||
|| One handler runs twice | The subscription was made more than once. There is no unsubscribing from interface events, so subscribe once per widget load ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../card.md)
- [{#T}](../webrtc-scenario.md)
- [{#T}](../../bx24-placement-bind-event.md)
- [{#T}](../../../universal/background-worker.md)
- [{#T}](../../../placement-bind.md)

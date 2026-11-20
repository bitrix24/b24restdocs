# Manage the Call Card of a WebRTC Client: Overview of Commands and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `PAGE_BACKGROUND_WORKER` placement is used by telephony applications that need to work with the call card from an invisible background frame on Bitrix24 pages. Through the JS interface of the placement, the application changes the state of the card, manages the operator's actions, and handles interface events.

{% note warning "" %}

The commands and events of this section are not REST methods. The application calls them in the browser through the JS interface of the placement: commands with the [BX24.placement.call](../bx24-placement-call.md) method, event subscriptions with the [BX24.placement.bindEvent](../bx24-placement-bind-event.md) method. There are no requests to `/rest/` with the names `CallCardSetUiState` or `BackgroundCallCard::initialized`.

{% endnote %}

> Quick navigation: [all commands and events](#all-methods)
>
> User documentation: [Call Card](https://helpdesk.bitrix24.com/open/18493064/)

## Getting Started with PAGE_BACKGROUND_WORKER

1. Register the `PAGE_BACKGROUND_WORKER` placement with the [placement.bind](../../placement-bind.md) method. The method works only from the application context: the placement cannot be bound with a webhook. Who can register widgets is described on the method page
2. During registration, pass the required `OPTIONS[errorHandlerUrl]` parameter. An application can have only one handler for this placement. If a common handler already exists, the method does not register a personal handler for an individual user and returns the `ERROR_PLACEMENT_MAX_COUNT` error. Details are in the article [{#T}](../../universal/background-worker.md)
3. Check the registration scenario and the requirements for the handler on the page [{#T}](./webrtc-scenario.md)
4. Register the call with the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method — the same method shows the call card. Subscribe to the card events in advance, in the code of the background handler: the handler does not receive events sent before the subscription
5. Wait for the [BackgroundCallCard::initialized](./events/initialized.md) event. Before it, there is no card, and the card management commands return the `Call card is undefined` error. Only [CallCardGetListUiStates](./call-card-get-list-ui-states.md) works without a card
6. Manage the card with commands through [BX24.placement.call](../bx24-placement-call.md) and subscribe to the operator's actions through [BX24.placement.bindEvent](../bx24-placement-bind-event.md)

The fourth step is mandatory. The commands and events work only with the card of an external call shown by the `telephony.externalCall.register` method. During a regular Bitrix24 call, the card looks the same, but the `BackgroundCallCard::*` events do not arrive, and the commands respond with the `Call card is undefined` error.

A minimal working cycle: the application waits for the card to be created, switches it to the required state, and listens to the operator's buttons.

```js
BX24.ready(function () {
    BX24.init(function () {
        BX24.placement.bindEvent('BackgroundCallCard::initialized', function (eventData) {
            // the card is created — now it can be managed
            BX24.placement.call('CallCardSetUiState', {uiState: 'connected'}, function (result) {
                console.log(result);
            });
        });

        BX24.placement.bindEvent('BackgroundCallCard::hangupButtonClick', function () {
            // the operator clicked "Hang Up" — end the call in your own WebRTC client
        });
    });
});
```

## How a Placement Command Call Works

A command is invoked by name: pass the command name as the first argument of [BX24.placement.call](../bx24-placement-call.md), the parameters object as the second, and the callback function as the third. If a command has no parameters, pass an empty object `{}` as the second argument. The parameters each command accepts are shown in the table of the [Commands Overview](#all-methods).

The result depends on the command.

- The card management commands — `CallCardSetMute`, `CallCardSetHold`, `CallCardSetUiState`, `CallCardSetCardTitle`, `CallCardSetStatusText`, `CallCardClose` — pass an empty array to the callback function
- `CallCardGetListUiStates` returns an array of the available interface states. This command works even without an active card and has no error codes of its own
- In the browser, `CallCardStartTimer` and `CallCardStopTimer` do not invoke the callback function at all: on success, there is no result. In the Bitrix24 desktop application, they return an empty array

If there is no call card, an array with the object `{result: 'error', errorCode: 'Call card is undefined'}` arrives instead of the result. If the command was invoked in another placement, the callback function is not invoked at all — the placement interface ignores an unknown command.

Some checks are performed only by the Bitrix24 desktop application: the codes `missing field muted`, `missing field held`, `missing field title`, and `missing field statusText` come from it. The format of `missing field muted` differs — this code arrives as a single object, without being wrapped in an array.

Events arrive in the callback function passed to [BX24.placement.bindEvent](../bx24-placement-bind-event.md). The handler receives either the data of the call and the card or the data of a specific operator action — the content is described on the page of each event.

How the card itself is arranged, which areas it has, and which buttons are available in each state is shown on the page [{#T}](./card.md).

## Overview of Commands and Events {#all-methods}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can execute the commands: an employee to whom Bitrix24 showed the call card, if they have access to the application

{% list tabs %}

- Commands

    #|
    || **Command** | **Description** | **Parameters** ||
    || [CallCardSetMute](./call-card-set-mute.md) | Toggles the microphone button in the card | `muted` ||
    || [CallCardSetHold](./call-card-set-hold.md) | Toggles the hold button in the card | `held` ||
    || [CallCardSetUiState](./call-card-set-ui-state.md) | Changes the interface state of the call card | `uiState`, optional `disableAutoStartTimer` ||
    || [CallCardGetListUiStates](./call-card-get-list-ui-states.md) | Returns the available interface states of the card | none ||
    || [CallCardSetCardTitle](./call-card-set-card-title.md) | Changes the title of the call card | `title` ||
    || [CallCardSetStatusText](./call-card-set-status-text.md) | Changes the status text below the client data | `statusText` ||
    || [CallCardStartTimer](./call-card-start-timer.md) | Shows the timer and starts counting the conversation time | none ||
    || [CallCardStopTimer](./call-card-stop-timer.md) | Stops counting the conversation time | none ||
    || [CallCardClose](./call-card-close.md) | Closes the call card | none ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [BackgroundCallCard::initialized](./events/initialized.md) | After the call card is created ||
    || [BackgroundCallCard::addCommentButtonClick](./events/add-comment-button-click.md) | When a comment is saved in the call card ||
    || [BackgroundCallCard::muteButtonClick](./events/mute-button-click.md) | When the microphone button is clicked ||
    || [BackgroundCallCard::holdButtonClick](./events/hold-button-click.md) | When the hold call button is clicked ||
    || [BackgroundCallCard::closeButtonClick](./events/close-button-click.md) | When the close call card button is clicked ||
    || [BackgroundCallCard::transferButtonClick](./events/transfer-button-click.md) | When an operator is selected to transfer the call to ||
    || [BackgroundCallCard::cancelTransferButtonClick](./events/cancel-transfer-button-click.md) | When the "continue call" button is clicked ||
    || [BackgroundCallCard::completeTransferButtonClick](./events/complete-transfer-button-click.md) | When the "redirect" button is clicked ||
    || [BackgroundCallCard::hangupButtonClick](./events/hang-up-button-click.md) | When the "hang up" button is clicked ||
    || [BackgroundCallCard::nextButtonClick](./events/next-button-click.md) | When the "next" button is clicked. In the current version, it does not arrive in the application ||
    || [BackgroundCallCard::skipButtonClick](./events/skip-button-click.md) | When the "ignore" button is clicked ||
    || [BackgroundCallCard::answerButtonClick](./events/answer-button-click.md) | When the "answer" button is clicked ||
    || [BackgroundCallCard::entityChanged](./events/entity-changed.md) | When the CRM object linked to the call is loaded or changed ||
    || [BackgroundCallCard::makeCallButtonClick](./events/make-call-button-click.md) | When the "call" or "redial" button is clicked ||
    || [BackgroundCallCard::dialpadButtonClick](./events/dialpad-button-click.md) | When one of the numeric buttons of the phone is pressed ||
    || [BackgroundCallCard::notifyAdminButtonClick](./events/notify-admin-button-click.md) | When the "notify administrator" button is clicked ||
    |#

{% endlist %}

## Typical Errors

#|
|| **Problem** | **Cause and what to do** ||
|| The command returns `Call card is undefined` | There is no call card, or the call was not registered with the `telephony.externalCall.register` method. Invoke commands only after the [BackgroundCallCard::initialized](./events/initialized.md) event ||
|| The callback function is not invoked at all | The widget is open outside the `PAGE_BACKGROUND_WORKER` placement, or the command name is misspelled. Check the list of available commands with the [BX24.placement.getInterface](../bx24-placement-get-interface.md) method. For the `CallCardStartTimer` and `CallCardStopTimer` commands, silence in the browser is the normal behavior on success ||
|| The widget is in the right placement, but events do not arrive and commands return `Call card is undefined` | The call is not an external one. The card has to be raised by the handler itself with the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method; this interface does not work for calls made by Bitrix24 itself ||
|| The `CallCardSetUiState` command returns `Invalid ui state` | The state is not in the list of supported ones. Retrieve the list with the [CallCardGetListUiStates](./call-card-get-list-ui-states.md) command ||
|| The handler stopped being invoked, the registration disappeared | The widget responded for longer than five seconds more than ten times a day, and Bitrix24 deleted the registration, reporting it to `OPTIONS[errorHandlerUrl]`. Details are in the article [{#T}](../../universal/background-worker.md) ||
|#

## Continue Learning

- [{#T}](./webrtc-scenario.md)
- [{#T}](./card.md)
- [{#T}](./events/index.md)
- [{#T}](../../universal/background-worker.md)
- [{#T}](../index.md)

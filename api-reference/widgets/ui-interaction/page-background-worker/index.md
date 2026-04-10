# Managing the Call Card of a WebRTC Client: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The `PAGE_BACKGROUND_WORKER` placement is utilized by telephony applications that need to interact with the call card from a background invisible frame on Bitrix24 pages. Through the JavaScript interface of the placement, the application can change the state of the card, manage operator actions, and handle interface events.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Call Card](https://helpdesk.bitrix24.com/open/18493064/)

## Getting Started with PAGE_BACKGROUND_WORKER

1. Register the `PAGE_BACKGROUND_WORKER` placement via [placement.bind](../../placement-bind.md).
2. Check the registration script and requirements for `errorHandlerUrl` on the [WebRTC Integration Script](./webrtc-scenario.md) page.
3. Register the call using [telephony.externalcall.register](../../../telephony/index.md) to open the call card.
4. After the [BackgroundCallCard::initialized](./events/initialized.md) event, invoke placement commands through [BX24.placement.call](../bx24-placement-call.md).
5. Subscribe to user actions on the card via [BX24.placement.bindEvent](../bx24-placement-bind-event.md).

## How the Placement API Call Works

In `PAGE_BACKGROUND_WORKER`, placement commands and events are not standalone REST methods. The application interacts with them through a common JavaScript embedding interface.

To execute a placement command, pass its name to [BX24.placement.call](../bx24-placement-call.md). For `PAGE_BACKGROUND_WORKER`, the available commands are `CallCardSetMute`, `CallCardSetHold`, `CallCardSetUiState`, `CallCardGetListUiStates`, `CallCardSetCardTitle`, `CallCardSetStatusText`, and `CallCardClose`. Card management commands typically return an empty array upon successful invocation, while `CallCardGetListUiStates` returns an array of available interface states.

To respond to user actions on the card without making repeated requests, subscribe to events via [BX24.placement.bindEvent](../bx24-placement-bind-event.md). In `PAGE_BACKGROUND_WORKER`, a set of events `BackgroundCallCard::*` is available from the [Events](./events/index.md) section. The handler receives data about the current call and card, as well as specific user action data from the interface.

## Overview of Methods and Events {#all-methods}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute methods: any user

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [CallCardSetMute](./call-card-set-mute.md) | Mutes or unmutes the operator's microphone ||
    || [CallCardSetHold](./call-card-set-hold.md) | Puts the call on hold or resumes it ||
    || [CallCardSetUiState](./call-card-set-ui-state.md) | Changes the state of the call card interface ||
    || [CallCardGetListUiStates](./call-card-get-list-ui-states.md) | Returns available states of the card interface ||
    || [CallCardSetCardTitle](./call-card-set-card-title.md) | Changes the title of the call card ||
    || [CallCardSetStatusText](./call-card-set-status-text.md) | Changes the text in the central part of the card ||
    || [CallCardClose](./call-card-close.md) | Closes the call card ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [BackgroundCallCard::initialized](./events/initialized.md) | After the call card is created ||
    || [BackgroundCallCard::addCommentButtonClick](./events/add-comment-button-click.md) | When saving a comment in the call card ||
    || [BackgroundCallCard::muteButtonClick](./events/mute-button-click.md) | When the mute button is clicked ||
    || [BackgroundCallCard::holdButtonClick](./events/hold-button-click.md) | When the hold button is clicked ||
    || [BackgroundCallCard::closeButtonClick](./events/close-button-click.md) | When the close button of the call card is clicked ||
    || [BackgroundCallCard::transferButtonClick](./events/transfer-button-click.md) | When selecting an operator to transfer the call ||
    || [BackgroundCallCard::cancelTransferButtonClick](./events/cancel-transfer-button-click.md) | When the "return to call" button is clicked ||
    || [BackgroundCallCard::completeTransferButtonClick](./events/complete-transfer-button-click.md) | When the "redirect" button is clicked ||
    || [BackgroundCallCard::hangupButtonClick](./events/hang-up-button-click.md) | When the "end" button is clicked ||
    || [BackgroundCallCard::nextButtonClick](./events/next-button-click.md) | When the "next" button is clicked ||
    || [BackgroundCallCard::skipButtonClick](./events/skip-button-click.md) | When the "skip" button is clicked ||
    || [BackgroundCallCard::answerButtonClick](./events/answer-button-click.md) | When the "answer" button is clicked ||
    || [BackgroundCallCard::entityChanged](./events/entity-changed.md) | When the current entity being called changes in the call card ||
    || [BackgroundCallCard::makeCallButtonClick](./events/make-call-button-click.md) | When the "call" or "call back" button is clicked ||
    || [BackgroundCallCard::qualityMeterClick](./events/quality-meter-click.md) | When rating the call quality ||
    || [BackgroundCallCard::dialpadButtonClick](./events/dialpad-button-click.md) | When one of the phone's number buttons is clicked ||
    || [BackgroundCallCard::notifyAdminButtonClick](./events/notify-admin-button-click.md) | When the "notify admin" button is clicked ||
    |#

{% endlist %}
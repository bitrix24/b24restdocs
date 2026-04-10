# Overview of Events When Working with the WebRTC Client Call Card

The `BackgroundCallCard::*` events allow the application to respond to user actions in the call card and changes in the interface state without additional polling.

{% note info "" %}

Events only work within the application in the `PAGE_BACKGROUND_WORKER` placement.

{% endnote %}

> Quick navigation: [all events](#all-events)

## How to Subscribe to Events

1. Subscribe to the desired event using [BX24.placement.bindEvent](../../bx24-placement-bind-event.md).
2. Handle the event data in the application's callback.
3. After the [BackgroundCallCard::initialized](./initialized.md) event, initiate the call card management logic.

## How to Choose an Event

#| 
|| **Scenario** | **What to Use** | **What Comes in the Callback** ||
|| The application has access to the created call card | [BackgroundCallCard::initialized](initialized.md) | Initial call data and CRM binding ||
|| The operator clicks call control buttons | [BackgroundCallCard::muteButtonClick](mute-button-click.md), [BackgroundCallCard::holdButtonClick](hold-button-click.md), [BackgroundCallCard::hangupButtonClick](hang-up-button-click.md), [BackgroundCallCard::answerButtonClick](answer-button-click.md) | Data for the specific action in the interface ||
|| The operator is working with call transfer | [BackgroundCallCard::transferButtonClick](transfer-button-click.md), [BackgroundCallCard::cancelTransferButtonClick](cancel-transfer-button-click.md), [BackgroundCallCard::completeTransferButtonClick](complete-transfer-button-click.md) | Data for the transfer scenario ||
|| In dialing mode, the current client changes | [BackgroundCallCard::entityChanged](entity-changed.md) | Client number and current CRM binding ||
|| Additional actions are needed from the call card interface, such as saving a comment, rating call quality, or entering a number on the keypad | [BackgroundCallCard::addCommentButtonClick](add-comment-button-click.md), [BackgroundCallCard::dialpadButtonClick](dialpad-button-click.md), [BackgroundCallCard::qualityMeterClick](quality-meter-click.md), [BackgroundCallCard::notifyAdminButtonClick](notify-admin-button-click.md), [BackgroundCallCard::nextButtonClick](next-button-click.md), [BackgroundCallCard::skipButtonClick](skip-button-click.md), [BackgroundCallCard::makeCallButtonClick](make-call-button-click.md), [BackgroundCallCard::closeButtonClick](close-button-click.md) | Value selected by the user or action parameters ||
|#

## Overview of Events {#all-events}

> Scope: [`telephony`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered** ||
|| [BackgroundCallCard::initialized](initialized.md) | After the call card is created ||
|| [BackgroundCallCard::addCommentButtonClick](add-comment-button-click.md) | When saving a comment in the call card ||
|| [BackgroundCallCard::muteButtonClick](mute-button-click.md) | When the mute button is pressed ||
|| [BackgroundCallCard::holdButtonClick](hold-button-click.md) | When the hold button is pressed ||
|| [BackgroundCallCard::closeButtonClick](close-button-click.md) | When the close button on the call card is pressed ||
|| [BackgroundCallCard::transferButtonClick](transfer-button-click.md) | When selecting the operator to whom the current operator wants to transfer the call ||
|| [BackgroundCallCard::cancelTransferButtonClick](cancel-transfer-button-click.md) | When the "return to call" button is pressed ||
|| [BackgroundCallCard::completeTransferButtonClick](complete-transfer-button-click.md) | When the "redirect" button is pressed ||
|| [BackgroundCallCard::hangupButtonClick](hang-up-button-click.md) | When the "end" button is pressed ||
|| [BackgroundCallCard::nextButtonClick](next-button-click.md) | When the "next" button is pressed ||
|| [BackgroundCallCard::skipButtonClick](skip-button-click.md) | When the "skip" button is pressed ||
|| [BackgroundCallCard::answerButtonClick](answer-button-click.md) | When the "answer" button is pressed ||
|| [BackgroundCallCard::entityChanged](entity-changed.md) | In the dialing card when the current dialed entity changes ||
|| [BackgroundCallCard::makeCallButtonClick](make-call-button-click.md) | When the "call" or "callback" button is pressed ||
|| [BackgroundCallCard::qualityMeterClick](quality-meter-click.md) | When rating call quality ||
|| [BackgroundCallCard::dialpadButtonClick](dialpad-button-click.md) | When one of the numeric buttons on the phone is pressed ||
|| [BackgroundCallCard::notifyAdminButtonClick](notify-admin-button-click.md) | When the "notify administrator" button is pressed ||
|#
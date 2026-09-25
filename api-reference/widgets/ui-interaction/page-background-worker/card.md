# Call Card

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can execute the commands: the employee to whom Bitrix24 displays the card, provided they have access to the application

The call card is the window in which the operator sees a call and manages it: answers it, ends the conversation, puts it on hold, or transfers it to a colleague. A telephony application with its own WebRTC client changes the title, the status text, and the set of buttons of the card. The application learns about button clicks from events and performs the call action itself, for example, ends the call in its own client. Bitrix24 also handles the Close button on its own: the button sends an event and closes the card.

Only the card of an external call can be managed. The application registers the call with the [telephony.externalCall.register](../../../telephony/telephony-external-call-register.md) method, and Bitrix24 displays the card to the operator — the employee specified in the `USER_ID` or `USER_PHONE_INNER` parameter of that method. Whether the card is displayed depends on the `SHOW` parameter: it defaults to `1`, and with `0` the card appears only after the [telephony.externalCall.show](../../../telephony/telephony-external-call-show.md) method is called. Commands and events go through the background handler in the `PAGE_BACKGROUND_WORKER` placement.

The card management commands start working after the [BackgroundCallCard::initialized](./events/initialized.md) event, which arrives when the card is created. Before this event, after the card is closed, and during a regular Bitrix24 call, they return the `Call card is undefined` error. The [CallCardGetListUiStates](./call-card-get-list-ui-states.md) command works even without a card.

For instructions on registering a handler, see [{#T}](./webrtc-scenario.md). All commands and events are listed in the section overview [{#T}](./index.md).

## Card Structure

To change the title at the top of the card, call the [CallCardSetCardTitle](./call-card-set-card-title.md) command and pass an object with the `title` property.

```js
BX24.placement.call('CallCardSetCardTitle', { title: 'Card Title' }, (result) => {
    console.log(result); // [] — the title is changed
});
```

To change the status text below the name of the other party, call the [CallCardSetStatusText](./call-card-set-status-text.md) command and pass an object with the `statusText` property.

```js
BX24.placement.call('CallCardSetStatusText', { statusText: 'Status Text' }, (result) => {
    console.log(result); // [] — the text is changed
});
```

The buttons at the bottom of the card depend on the interface state. The [CallCardGetListUiStates](./call-card-get-list-ui-states.md) command returns 12 states to which the application can switch the card: the callback function receives an array of their codes.

```js
BX24.placement.call('CallCardGetListUiStates', {}, (data) => {
    console.log(data);
});
```

The transition to another state of the card is performed by the [CallCardSetUiState](./call-card-set-ui-state.md) command: pass it an object with the `uiState` property.

```js
BX24.placement.call('CallCardSetUiState', { uiState: 'connected' }, (result) => {
    console.log(result); // [] — the state is changed
});
```

To find out which buttons the operator clicks, subscribe to events with the [BX24.placement.bindEvent](../bx24-placement-bind-event.md) method. The table below shows which event each button sends, and the events are described in the section [{#T}](./events/index.md).

## Card States

Right after it appears, the card is in an internal state: it has only the Close button at the bottom. This state is not listed in the table below or in the response of the [CallCardGetListUiStates](./call-card-get-list-ui-states.md) command. To show the operator the required buttons, switch the card to one of the states with the [CallCardSetUiState](./call-card-set-ui-state.md) command.

#|
|| **State** | **When Used** | **Buttons and Events** ||
|| incoming | For accepting incoming calls |
- Answer — [BackgroundCallCard::answerButtonClick](./events/answer-button-click.md)
- Skip — [BackgroundCallCard::skipButtonClick](./events/skip-button-click.md) ||
|| transferIncoming | For accepting a redirected incoming call |
- Answer — [BackgroundCallCard::answerButtonClick](./events/answer-button-click.md)
- Skip — [BackgroundCallCard::skipButtonClick](./events/skip-button-click.md) ||
|| outgoing | For displaying the outgoing call card |
- Call — [BackgroundCallCard::makeCallButtonClick](./events/make-call-button-click.md) ||
|| connectingIncoming | For displaying the card while connecting to an incoming call |
- Hang up — [BackgroundCallCard::hangupButtonClick](./events/hang-up-button-click.md) ||
|| connectingOutgoing | For displaying the card while connecting to an outgoing call |
- Hang up — [BackgroundCallCard::hangupButtonClick](./events/hang-up-button-click.md) ||
|| connected | For displaying after connecting to the call |
- Hang up — [BackgroundCallCard::hangupButtonClick](./events/hang-up-button-click.md)
- Hold — [BackgroundCallCard::holdButtonClick](./events/hold-button-click.md)
- Mute — [BackgroundCallCard::muteButtonClick](./events/mute-button-click.md)
- Transfer to another operator — [BackgroundCallCard::transferButtonClick](./events/transfer-button-click.md), the event arrives after an employee is selected
- Pressing buttons on the dial pad — [BackgroundCallCard::dialpadButtonClick](./events/dialpad-button-click.md) ||
|| transferring | For confirming the transfer of the call to another operator |
- Transfer — [BackgroundCallCard::completeTransferButtonClick](./events/complete-transfer-button-click.md)
- Return to call — [BackgroundCallCard::cancelTransferButtonClick](./events/cancel-transfer-button-click.md) ||
|| transferFailed | If the call transfer failed |
- Return to call — [BackgroundCallCard::cancelTransferButtonClick](./events/cancel-transfer-button-click.md) ||
|| transferConnected | If the transfer was successful and you need to exit the call card |
- Hang up — [BackgroundCallCard::hangupButtonClick](./events/hang-up-button-click.md) ||
|| error | If a call error occurred |
- Close — [BackgroundCallCard::closeButtonClick](./events/close-button-click.md) ||
|| moneyError | If the account runs out of money and you need to inform the Bitrix24 administrator |
- Notify administrator — [BackgroundCallCard::notifyAdminButtonClick](./events/notify-admin-button-click.md)
- Close — [BackgroundCallCard::closeButtonClick](./events/close-button-click.md) ||
|| redial | If the subscriber is busy and the operator needs to call this number again without closing the call card |
- Call again — [BackgroundCallCard::makeCallButtonClick](./events/make-call-button-click.md) ||
|#

The call quality indicator in the `connected` state does not accept clicks and does not send events.

### Call Timer

When the card switches to the `connected` state, the call timer starts automatically. To prevent it from starting, pass the `disableAutoStartTimer: true` property along with `uiState: 'connected'`.

In the call transfer states — `transferring`, `transferFailed`, and `transferConnected` — the timer keeps running. All other states except `connected` stop the timer. You can stop or restart the timer with the [CallCardStopTimer](./call-card-stop-timer.md) and [CallCardStartTimer](./call-card-start-timer.md) commands.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./events/index.md)
- [{#T}](./call-card-set-ui-state.md)
- [{#T}](./webrtc-scenario.md)

# Call Card

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The call card consists of areas: the title, the status text, and the operator buttons. Each area is changed by its own command, and the set of buttons depends on the interface state — there are twelve of them in total. The state also determines which events reach the application.

The commands and events that manage the card are listed in the section overview [{#T}](./index.md), and the order of registering a handler is described on the page [{#T}](./webrtc-scenario.md).

## General Description

To change the title of the card (area 1), call the [CallCardSetCardTitle](./call-card-set-card-title.md) command and pass an object with the `title` property.

```js
BX24.placement.call('CallCardSetCardTitle', { title: 'Card Title' }, () => {
    // some code
});
```

To change the text in area 2, call the [CallCardSetStatusText](./call-card-set-status-text.md) command and pass an object with the `statusText` property.

```js
BX24.placement.call('CallCardSetStatusText', { statusText: 'Status Text' }, () => {
    // some code
});
```

The call card has a total of 12 interface states. You can retrieve them with the [CallCardGetListUiStates](./call-card-get-list-ui-states.md) command. An array of available call card states will be passed to the callback function.

```js
BX24.placement.call('CallCardGetListUiStates', {}, (data) => {
    console.log(data);
});
```

The transition to another state of the card is performed by the [CallCardSetUiState](./call-card-set-ui-state.md) command: pass it an object with the `uiState` property.

```js
BX24.placement.call('CallCardSetUiState', { uiState: 'connected' }, () => {
    // some code
});
```

To handle button presses by the operator in the call card, subscribe to the corresponding events — see [{#T}](./events/index.md).

## Card States

#|
|| **State** | **Description** | **Handles Button Presses** ||
|| incoming | For accepting incoming calls |
- Answer - `BackgroundCallCard::answerButtonClick`
- Skip - `BackgroundCallCard::skipButtonClick` ||
|| transferIncoming | For accepting a redirected incoming call |
- Answer - `BackgroundCallCard::answerButtonClick`
- Skip - `BackgroundCallCard::skipButtonClick` ||
|| outgoing | For displaying the outgoing call card |
- Call - `BackgroundCallCard::makeCallButtonClick` ||
|| connectingIncoming | For displaying the card while connecting to an incoming call |
- Hang up - `BackgroundCallCard::hangupButtonClick` ||
|| connectingOutgoing | For displaying the card while connecting to an outgoing call |
- Hang up - `BackgroundCallCard::hangupButtonClick` ||
|| connected | For displaying after connecting to the call |
- Hang up - `BackgroundCallCard::hangupButtonClick`
- Hold - `BackgroundCallCard::holdButtonClick`
- Mute - `BackgroundCallCard::muteButtonClick`
- Transfer to another operator - `BackgroundCallCard::transferButtonClick`
- Pressing buttons on the dial pad - `BackgroundCallCard::dialpadButtonClick`
- Rate call quality - `BackgroundCallCard::qualityMeterClick` ||
|| transferring | For confirming the transfer of the call to another operator |
- Transfer - `BackgroundCallCard::completeTransferButtonClick`
- Return to call - `BackgroundCallCard::cancelTransferButtonClick` ||
|| transferFailed | If the call transfer failed |
- Return to call - `BackgroundCallCard::cancelTransferButtonClick` ||
|| transferConnected | If the transfer was successful and you need to exit the call card |
- Hang up - `BackgroundCallCard::hangupButtonClick` ||
|| error | If an error occurred |
- Close - `BackgroundCallCard::closeButtonClick` ||
|| moneyError | If the account runs out of money and you need to inform the Bitrix24 administrator |
- Notify administrator - `BackgroundCallCard::notifyAdminButtonClick`
- Close - `BackgroundCallCard::closeButtonClick` ||
|| redial | If the subscriber is busy, allow the operator to call this number again without hiding the call card |
- Call again - `BackgroundCallCard::makeCallButtonClick` ||
|| Timer in the call card | By default, when transitioning to the `connected` state, the call timer is automatically started. This behavior can be disabled by passing, in addition to `uiState: 'connected'`, the property `disableAutoStartTimer` with the value `true`. When transitioning to other states, the timer will stop. | ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./events/index.md)
- [{#T}](./call-card-set-ui-state.md)
- [{#T}](./webrtc-scenario.md)

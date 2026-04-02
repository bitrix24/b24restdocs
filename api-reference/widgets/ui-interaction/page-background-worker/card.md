# Call Card

To manage the call card, it is advisable to familiarize yourself with the [scenarios](index.md).

## General Description

Let's take a look at the call card:


To change the title of the card (area 1), you need to call the method [CallCardSetCardTitle](./call-card-set-card-title.md) and pass an object with the `title` property.

```js
BX24.placement.call('CallCardSetCardTitle', { title: 'Card Title' }, () => {
    // some code
});
```

To change the text in area 2, you need to call the method [CallCardSetStatusText](./call-card-set-status-text.md) and pass an object with the `statusText` property.

```js
BX24.placement.call('CallCardSetStatusText', { statusText: 'Status Text' }, () => {
    // some code
});
```

The call card has a total of 12 interface states. You can retrieve them by calling the method [CallCardGetListUiStates](./call-card-get-list-ui-states.md). An array of available call card states will be passed to the callback function.

```js
BX24.placement.call('CallCardGetListUiStates', {}, (data) => {
    console.log(data);
});
```

To switch to another state of the card, you call the method [CallCardSetUiState](./call-card-set-ui-state.md) and pass an object with the `uiState` property.

```js
BX24.placement.call('CallCardSetUiState', { uiState: 'connected' }, () => {
    // some code
});
```

To handle button presses by the operator in the call card, you need to subscribe to the corresponding events.

## Card States

#|
|| **State** | **Description** | **Handles Button Presses** ||
|| [incoming](*incoming) | For accepting incoming calls |
- Answer - `BackgroundCallCard::answerButtonClick`
- Skip - `BackgroundCallCard::skipButtonClick` ||
|| [transferIncoming](*transferIncoming) | For accepting a redirected incoming call |
- Answer - `BackgroundCallCard::answerButtonClick`
- Skip - `BackgroundCallCard::skipButtonClick` ||
|| [outgoing](*outgoing) | For displaying the outgoing call card |
- Call - `BackgroundCallCard::makeCallButtonClick` ||
|| [connectingIncoming](*connectingIncoming) | For displaying the card while connecting to an incoming call |
- Hang up - `BackgroundCallCard::hangupButtonClick` ||
|| [connectingOutgoing](*connectingOutgoing) | For displaying the card while connecting to an outgoing call |
- Hang up - `BackgroundCallCard::hangupButtonClick` ||
|| [connected](*connected) | For displaying after connecting to the call |
- Hang up - `BackgroundCallCard::hangupButtonClick`
- Hold - `BackgroundCallCard::holdButtonClick`
- Mute - `BackgroundCallCard::muteButtonClick`
- Transfer to another operator - `BackgroundCallCard::transferButtonClick`
- Pressing buttons on the dial pad - `BackgroundCallCard::dialpadButtonClick`
- Rate call quality - `BackgroundCallCard::qualityMeterClick` ||
|| [transferring](*transferring) | For confirming the transfer of the call to another operator |
- Transfer - `BackgroundCallCard::completeTransferButtonClick`
- Return to call - `BackgroundCallCard::cancelTransferButtonClick` ||
|| [transferFailed](*transferFailed) | If the call transfer failed |
- Return to call - `BackgroundCallCard::cancelTransferButtonClick` ||
|| [transferConnected](*transferConnected) | If the transfer was successful and you need to exit the call card |
- Hang up - `BackgroundCallCard::hangupButtonClick` ||
|| [error](*error) | If an error occurred |
- Close - `BackgroundCallCard::closeButtonClick` ||
|| [moneyError](*moneyError) | If the account runs out of money and you need to inform the account administrator |
- Notify administrator - `BackgroundCallCard::notifyAdminButtonClick`
- Close - `BackgroundCallCard::closeButtonClick` ||
|| [redial](*redial) | If the subscriber is busy, allow the operator to call this number again without hiding the call card |
- Call again - `BackgroundCallCard::makeCallButtonClick` ||
|| Timer in the call card | By default, when transitioning to the `connected` state, the call timer is automatically started. This behavior can be disabled by passing, in addition to `uiState: 'connected'`, the property `disableAutoStartTimer` with the value `true`. When transitioning to other states, the timer will stop. | ||
|#












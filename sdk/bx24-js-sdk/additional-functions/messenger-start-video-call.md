# Start Video Call Messenger.startVideoCall

The method `Messenger.startVideoCall` initiates a video call in Bitrix24. It is recommended to use this method instead of `BX24.im.callTo`.

```js
Promise Messenger.startVideoCall([String dialogId[, Boolean withVideo]])
```

## Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **dialogId**
`string` | Identifier of the dialog or chat. By default, an empty string `''` is used. An empty or invalid value will not pass validation, and the call will terminate without starting the call ||
|| **withVideo**
`boolean` | Type of call: `true` - video call, `false` - audio call ||
|#

## Code Example

{% include [Example Notes](../../../_includes/examples.md) %}

```js
BX.Messenger.Public.startVideoCall('chat123', true);
```

## Response Handling

The method returns a `Promise`.

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
`Promise` | Promise of the operation to initiate the video call ||
|#

## Continue Learning

- [{#T}](./messenger-start-phone-call.md)
- [{#T}](./outdated/bx24-im-call-to.md)
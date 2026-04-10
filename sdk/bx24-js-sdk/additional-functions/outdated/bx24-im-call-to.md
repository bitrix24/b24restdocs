# Call via Internal Communication BX24.im.callTo

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Messenger.startVideoCall](../messenger-start-video-call.md).

{% endnote %}

The method `BX24.im.callTo` sends a command to call a Bitrix24 user via internal communication.

```js
void BX24.im.callTo(Integer userId[, Boolean video])
```

## Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **userId*** 
`integer` | The identifier of the Bitrix24 user to be called ||
|| **video** 
`boolean` | Type of call: `true` - video call, `false` - audio call ||
|#

## Code Example

{% include [Note on examples](../../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.im.callTo(1, true);
});
```

## Response Handling

The method does not return data (`void`).

## Continue Learning

- [{#T}](./bx24-im-phone-to.md)
- [{#T}](./bx24-im-open-messenger.md)
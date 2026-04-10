# Open the BX24.im.openHistory Window

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Messenger.openChat](../messenger-open-chat.md).

{% endnote %}

The method `BX24.im.openHistory` sends a command to open the history window of a dialog.

```js
void BX24.im.openHistory(String dialogId)
```

## Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **dialogId*** 
`string` | Identifier of the dialog. Supported formats: `userId` or `chatXXX` for chat, `imol|XXXX` for Open Channels ||
|#

## Code Example

{% include [Note on examples](../../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.im.openHistory('chat123');
});
```

## Response Handling

The method does not return any data (`void`).

## Continue Learning

- [{#T}](./bx24-im-open-messenger.md)
- [{#T}](./bx24-im-call-to.md)
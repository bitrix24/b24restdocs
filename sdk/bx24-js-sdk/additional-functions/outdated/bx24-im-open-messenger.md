# Open Messenger Window BX24.im.openMessenger

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Messenger.openChat](../messenger-open-chat.md).

{% endnote %}

The method `BX24.im.openMessenger` sends a command to open the messenger window.

```js
void BX24.im.openMessenger([String dialogId])
```

## Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **dialogId**
`string` | Identifier of the dialog. Supported formats: `userId` or `chatXXX` for chat, `sgXXX` for group chat, `imol|XXXX` for Open Channels. If the parameter is not provided, the chat list interface will open. ||
|#

## Code Example

{% include [Example Note](../../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.im.openMessenger('chat123');
});
```

## Response Handling

The method does not return data (`void`).

## Continue Learning

- [{#T}](./bx24-im-open-history.md)
- [{#T}](./bx24-im-call-to.md)
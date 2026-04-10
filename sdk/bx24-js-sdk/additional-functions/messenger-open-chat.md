# Open Chat Messenger.openChat

The method `Messenger.openChat` opens a chat in the Bitrix24 Messenger interface. It is recommended to use this method instead of `BX24.im.openMessenger` and `BX24.im.openHistory`.

```js
Promise Messenger.openChat([String dialogId[, Integer messageId]])
```

## Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **dialogId**
`string` | Identifier of the dialog or chat. If this parameter is not provided, the chat list will open. ||
|| **messageId**
`integer` | Identifier of the message to open the chat focused on a specific message. ||
|#

## Code Example

{% include [Example Notes](../../../_includes/examples.md) %}

```js
BX.Messenger.Public.openChat('chat123');
```

```js
BX.Messenger.Public.openChat('chat123', 12345);
```

## Response Handling

The method returns a `Promise`.

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
`Promise` | Promise of the chat opening operation. ||
|#

## Continue Learning

- [{#T}](./outdated/bx24-im-open-messenger.md)
- [{#T}](./outdated/bx24-im-open-history.md)
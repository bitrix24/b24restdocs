# Send Message imbot.message.add

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chat bot. The method only works with bots from this application.

The method `imbot.message.add` sends a message from the chat bot.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **BOT_ID**
[`integer`](../../data-types.md) | The identifier of the chat bot. You can obtain the bot ID using the [imbot.bot.list](../imbot-bot-list.md) method.

If the parameter is not provided, the method searches for the first bot registered by the current application. ||
|| **DIALOG_ID**
[`string`](../../data-types.md) | The identifier of the object that will receive the message: user or chat.

Supported formats:
- `USER_ID` — the identifier of the user, which can be obtained via [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md)
- `chatXXX`, where `XXX` is the chat ID, which can be obtained via [imbot.chat.get](../chats/imbot-chat-get.md)

This parameter is required if both `FROM_USER_ID` and `TO_USER_ID` are not provided. ||

|| **FROM_USER_ID**
[`integer`](../../data-types.md) | The identifier of the sender user for sending in a private dialog. You can obtain the user ID using [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md).

Used only together with `TO_USER_ID`. If both parameters are provided and greater than `0`, the `DIALOG_ID` field is ignored, and `SYSTEM` is forcibly set to `Y`. ||
|| **TO_USER_ID**
[`integer`](../../data-types.md) | The identifier of the recipient user for sending in a private dialog. You can obtain the user ID using [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md).

Used only together with `FROM_USER_ID`. ||
|| **MESSAGE***
[`string`](../../data-types.md) | The text of the message. The method automatically trims whitespace and line breaks from the edges of the message text. ||
|| **ATTACH**
[`object`](../../data-types.md) 
[`string`](../../data-types.md) | An attachment with content blocks: images, links, files. You can pass:
- a JSON string
- an object with the root key `BLOCKS`
- an array of blocks without wrapping 

For more details, see the [Attachments](../../chats/messages/attachments/index.md) section. ||
|| **KEYBOARD**
[`object`](../../data-types.md) 
[`string`](../../data-types.md) | Buttons below the message that the user can interact with.

For more details, see the [Working with Keyboards](../../chats/messages/keyboards.md) article. ||
|| **MENU**
[`object`](../../data-types.md) 
[`string`](../../data-types.md) | Additional items in the chat's context menu.

For more details, see the [Context Menu](../../chats/messages/menu.md) article. ||
|| **SYSTEM**
[`string`](../../data-types.md) | Indicator of a system message.

Allowed values:
- `Y` — system message
- `N` — regular message, default value

In `FROM_USER_ID` + `TO_USER_ID` mode, the value is forcibly set to `Y`. ||
|| **URL_PREVIEW**
[`string`](../../data-types.md) | Controls the display of links: when enabled, the link is shown as a "rich link" with a card.

Allowed values:
- `Y` — enabled, default
- `N` — disabled ||
|| **SKIP_CONNECTOR**
[`string`](../../data-types.md) | Skip sending the message to external connectors of Open Channels.

Allowed values:
- `Y` — skip
- `N` — do not skip (default) ||
|| **CLIENT_ID**
[`string`](../../data-types.md) | Technical parameter for scenarios without `clientId` in authorization.

If provided, it is used as `custom{CLIENT_ID}` to identify the application. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"BOT_ID":39,"DIALOG_ID":"chat123","MESSAGE":"Message text","SYSTEM":"N","URL_PREVIEW":"Y"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"BOT_ID":39,"DIALOG_ID":"chat123","MESSAGE":"Message text","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.message.add
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.message.add', {
        BOT_ID: 39,
        DIALOG_ID: 'chat123',
        MESSAGE: 'Message text',
        URL_PREVIEW: 'Y',
      });

      const { result } = response.getData();
      console.log('Created message ID:', result);
    } catch (error) {
      console.error('Error sending message:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.message.add',
                [
                    'BOT_ID' => 39,
                    'DIALOG_ID' => 'chat123',
                    'MESSAGE' => 'Message text',
                    'URL_PREVIEW' => 'Y',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Created message ID: ' . $result->data();
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error sending message: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.message.add',
        {
            BOT_ID: 39,
            DIALOG_ID: 'chat123',
            MESSAGE: 'Message text',
            URL_PREVIEW: 'Y',
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.message.add',
        [
            'BOT_ID' => 39,
            'DIALOG_ID' => 'chat123',
            'MESSAGE' => 'Message text',
            'URL_PREVIEW' => 'Y',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Created message ID: ' . $result['result'];
    }
    ```
{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": 19880117,
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00",
        "operating_reset_at": 1762349466,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | The identifier of the created message. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MESSAGE_EMPTY",
    "error_description": "Message can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_ID_ERROR` | Bot not found | The bot is not found or the application does not have an available bot for auto-filling `BOT_ID`. ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | The provided `BOT_ID` belongs to another application. ||
|| `DIALOG_ID_EMPTY` | Dialog ID can't be empty | A valid `DIALOG_ID` was not provided if the pair `FROM_USER_ID` and `TO_USER_ID` is not used. ||
|| `MESSAGE_EMPTY` | Message can't be empty | The message text was not provided. ||
|| `ATTACH_OVERSIZE` | You have exceeded the maximum allowable size of attach | The maximum allowable size of `ATTACH` has been exceeded — 30 KB. ||
|| `ATTACH_ERROR` | Incorrect attach params | Invalid structure of `ATTACH`. ||
|| `KEYBOARD_ERROR` | Incorrect keyboard params | Invalid structure of `KEYBOARD`. ||
|| `KEYBOARD_OVERSIZE` | You have exceeded the maximum allowable size of keyboard | The maximum allowable size of `KEYBOARD` has been exceeded — 30 KB. ||
|| `MENU_ERROR` | Incorrect menu params | Invalid structure of `MENU`. ||
|| `MENU_OVERSIZE` | You have exceeded the maximum allowable size of menu | The maximum allowable size of `MENU` has been exceeded — 30 KB. ||
|| `WRONG_REQUEST` | Message isn't added | Internal error while adding the message. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-message-update.md)
- [{#T}](./imbot-message-delete.md)
- [{#T}](./imbot-message-like.md)
- [{#T}](./events/on-imbot-message-add.md)
- [{#T}](../../../tutorials/chat-bots/index.md)
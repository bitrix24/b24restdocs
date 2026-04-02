# Update Sent Message imbot.message.update

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chat bot. The method only works with bots from this application.

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Chat.Message.update](../../chat-bots-v2/imbot.v2/messages/chat-message-update.md).

{% endnote %}

The method `imbot.message.update` modifies a previously sent message from the chat bot.

## Method Parameters

{% include [Parameters Note](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **BOT_ID**
[`integer`](../../../data-types.md) | The identifier of the chat bot. It can be obtained using the [imbot.bot.list](../bots/imbot-bot-list.md) method.

If the parameter is not provided, the method searches for the first bot registered by the current application. ||
|| **MESSAGE_ID*** 
[`integer`](../../../data-types.md) | The identifier of the message to be modified. The value must be greater than `0`.

For messages sent by the bot via REST, the identifier is returned by the [imbot.message.add](./imbot-message-add.md) method. ||
|| **MESSAGE**
[`string`](../../../data-types.md) | The new text of the message. If an empty value is provided, the message will be deleted.

If the parameter is not provided, the message text will remain unchanged. ||
|| **ATTACH**
[`object`](../../../data-types.md)
[`string`](../../../data-types.md) | An attachment with content blocks: images, links, files. You can provide:
- a JSON string
- an object with the root key `BLOCKS`
- an array of blocks without wrapping 

For more details, refer to the [Attachments](../../../chats/messages/attachments.md) section.

An empty value or `N` clears the attachment. ||
|| **KEYBOARD**
[`object`](../../../data-types.md)
[`string`](../../../data-types.md) | Buttons under the message that the user can interact with.

For more information, see the article [Working with Keyboards](../../../chats/messages/keyboards.md).

An empty value or `N` disables the display of buttons. ||
|| **MENU**
[`object`](../../../data-types.md)
[`string`](../../../data-types.md) | Additional items in the chat's context menu.

For more details, refer to the article [Context Menu](../../../chats/messages/menu.md).

An empty value or `N` disables the display of additional items. ||
|| **URL_PREVIEW**
[`string`](../../../data-types.md) | Controls the display of links: when enabled, the link is shown as a "rich link" with a card.

Allowed values:
- `Y` — enabled, default value
- `N` — disabled ||
|| **SKIP_CONNECTOR**
[`string`](../../../data-types.md) | Skips sending the message to external connectors of open channels when updating `MESSAGE`.

Allowed values:
- `Y` — skip
- `N` — do not skip, default value ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified when registering the chat bot. ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
      -H "Content-Type: application/json" /
      -H "Accept: application/json" /
      -d '{"BOT_ID":39,"MESSAGE_ID":19880117,"MESSAGE":"Updated text","URL_PREVIEW":"Y","CLIENT_ID":"**put_your_client_id_here**"}' /
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.message.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
      -H "Content-Type: application/json" /
      -H "Accept: application/json" /
      -d '{"BOT_ID":39,"MESSAGE_ID":19880117,"MESSAGE":"Updated text","URL_PREVIEW":"Y","auth":"**put_access_token_here**"}' /
      https://**put_your_bitrix24_address**/rest/imbot.message.update
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.message.update', {
        BOT_ID: 39,
        MESSAGE_ID: 19880117,
        MESSAGE: 'Updated text',
        URL_PREVIEW: 'Y',
      });

      const { result } = response.getData();
      console.log('Updated:', result);
    } catch (error) {
      console.error('Error updating message:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.message.update',
                [
                    'BOT_ID' => 39,
                    'MESSAGE_ID' => 19880117,
                    'MESSAGE' => 'Updated text',
                    'URL_PREVIEW' => 'Y',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Updated: ' . ($result->data() ? 'true' : 'false');
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error updating message: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.message.update',
        {
            BOT_ID: 39,
            MESSAGE_ID: 19880117,
            MESSAGE: 'Updated text',
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
        'imbot.message.update',
        [
            'BOT_ID' => 39,
            'MESSAGE_ID' => 19880117,
            'MESSAGE' => 'Updated text',
            'URL_PREVIEW' => 'Y',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Updated: ' . ($result['result'] ? 'true' : 'false');
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": true,
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
[`boolean`](../../../data-types.md) | `true` if the request was processed without error. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "MESSAGE_ID_ERROR",
    "error_description": "Message ID can't be empty"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_ID_ERROR` | Bot not found | The bot is not found or the application does not have an available bot for auto-filling `BOT_ID`. ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | The provided `BOT_ID` belongs to another application. ||
|| `MESSAGE_ID_ERROR` | Message ID can't be empty | A valid `MESSAGE_ID` was not provided. ||
|| `ATTACH_ERROR` | Incorrect attach params | Invalid structure of `ATTACH`. ||
|| `ATTACH_OVERSIZE` | You have exceeded the maximum allowable size of attach | The allowable size of `ATTACH` has been exceeded — 30 KB. ||
|| `KEYBOARD_ERROR` | Incorrect keyboard params | Invalid structure of `KEYBOARD`. ||
|| `KEYBOARD_OVERSIZE` | You have exceeded the maximum allowable size of keyboard | The allowable size of `KEYBOARD` has been exceeded — 30 KB. ||
|| `MENU_ERROR` | Incorrect menu params | Invalid structure of `MENU`. ||
|| `MENU_OVERSIZE` | You have exceeded the maximum allowable size of menu | The allowable size of `MENU` has been exceeded — 30 KB. ||
|| `CANT_EDIT_MESSAGE` | Time has expired for modification or you don't have access | No access to modify the message or the editing time has expired — more than three days have passed since publication. ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Send Message imbot.message.add](./imbot-message-add.md)
- [Delete Message imbot.message.delete](./imbot-message-delete.md)
- [Set "Like" for the message imbot.message.like](./imbot-message-like.md)
- [Event on Message Update ONIMBOTMESSAGEUPDATE](./events/on-imbot-message-update.md)

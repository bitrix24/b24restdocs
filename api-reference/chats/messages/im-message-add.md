# Send Message im.message.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to send messages in chat

The method `im.message.add` sends a message to a chat.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the chat in the format:

- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — identifier of the personal chat user
  
The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method. The user identifier can be obtained using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods ||
|| **MESSAGE***
[`string`](../../data-types.md) | The text of the message. Required if `ATTACH` is not provided.

[Formatting](./formatting.md) is supported ||
|| **ATTACH**
[`object`](../../data-types.md) 
[`string`](../../data-types.md) | Attachment with content blocks: images, links, files. Required if `MESSAGE` is not provided.

Read more in the [Attachments](./attachments.md) section ||
|| **KEYBOARD**
[`object`](../../data-types.md) 
[`string`](../../data-types.md) | Buttons under the message that the user can interact with.

Read more in the [Working with Keyboards](./keyboards.md) article ||
|| **MENU**
[`object`](../../data-types.md) 
[`string`](../../data-types.md) | Additional items in the chat's context menu.

Read more in the [Context Menu](./menu.md) article ||
|| **SYSTEM**
[`string`](../../data-types.md) | Indicator of a system message.

Allowed values:
- `Y` — system message
- `N` — regular message
  
Default is `N` ||
|| **URL_PREVIEW**
[`string`](../../data-types.md) | Conversion of links into rich links.

Allowed values:
- `Y` — enabled
- `N` — disabled

Default is `Y` ||
|| **REPLY_ID**
[`integer`](../../data-types.md) | Identifier of the message to which the reply is sent. The message for the reply must be in the same chat.

The identifier can be obtained using the [im.dialog.messages.get](./im-dialog-messages-get.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2941","MESSAGE":"Message text","SYSTEM":"N","URL_PREVIEW":"Y","REPLY_ID":34237}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.message.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat2941","MESSAGE":"Message text","SYSTEM":"N","URL_PREVIEW":"Y","REPLY_ID":34237,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.message.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.message.add',
            {
                DIALOG_ID: 'chat2941',
                MESSAGE: 'Message text',
                SYSTEM: 'N',
                URL_PREVIEW: 'Y',
                REPLY_ID: 34237
            }
        );
        
        const result = response.getData().result;
        console.log('Created message with ID:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.message.add',
                [
                    'DIALOG_ID' => 'chat2941',
                    'MESSAGE' => 'Message text',
                    'SYSTEM' => 'N',
                    'URL_PREVIEW' => 'Y',
                    'REPLY_ID' => 34237
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.message.add',
        {
            DIALOG_ID: 'chat2941',
            MESSAGE: 'Message text',
            SYSTEM: 'N',
            URL_PREVIEW: 'Y',
            REPLY_ID: 34237
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.message.add',
        [
            'DIALOG_ID' => 'chat2941',
            'MESSAGE' => 'Message text',
            'SYSTEM' => 'N',
            'URL_PREVIEW' => 'Y',
            'REPLY_ID' => 34237
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 34239,
    "time": {
        "start": 1772552939,
        "finish": 1772552939.091396,
        "duration": 0.09139609336853027,
        "processing": 0,
        "date_start": "2026-03-03T17:48:59+01:00",
        "date_finish": "2026-03-03T17:48:59+01:00",
        "operating_reset_at": 1772553539,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the sent message ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MESSAGE_EMPTY",
    "error_description": "Message can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MESSAGE_EMPTY` | Message can't be empty | Empty `MESSAGE` and `ATTACH` not provided ||
|| `USER_ID_EMPTY` | User ID can't be empty | User identifier in `DIALOG_ID` is not provided or is invalid ||
|| `USER_NOT_FOUND` | User not found | User not found ||
|| `CHAT_ID` | Error creating message | Failed to create message. Ensure that a chat with such `ID` exists ||
|| `ACCESS_ERROR` | Action unavailable | Insufficient permissions to send the message ||
|| `PARAMS_ERROR` | Incorrect params | Invalid set of request parameters ||
|| `ATTACH_ERROR` | Incorrect attach params | Invalid `ATTACH` object ||
|| `ATTACH_OVERSIZE` | You have exceeded the maximum allowable size of attach | The size of `ATTACH` exceeds the allowable limit ||
|| `KEYBOARD_ERROR` | Incorrect keyboard params | Invalid `KEYBOARD` object ||
|| `MENU_ERROR` | Incorrect menu params | Invalid `MENU` object ||
|| `REPLY_ACCESS_ERROR` | Action unavailable | No access to the message in `REPLY_ID` ||
|| `REPLY_FROM_OTHER_CHAT_ERROR` | You can only reply to a message within the same chat | Cannot reply to a message from another chat ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-update.md)
- [{#T}](./im-message-delete.md)
- [{#T}](./im-message-like.md)
- [{#T}](./attachments.md)
- [{#T}](./keyboards.md)
- [{#T}](./menu.md)
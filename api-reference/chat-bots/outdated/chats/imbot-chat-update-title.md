# Change Chat Title imbot.chat.updateTitle

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chatbot

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Chat.update](../../chat-bots-v2/imbot.v2/chats/chat-update.md).

{% endnote %}

The method `imbot.chat.updateTitle` updates the chat title.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | The identifier of the chat.

The identifier can be obtained using the method [imbot.chat.get](./imbot-chat-get.md) ||
|| **TITLE***
[`string`](../../../data-types.md) | The new title of the chat.

The method automatically trims whitespace and line breaks from the value ||
|| **BOT_ID**
[`integer`](../../../data-types.md) | The identifier of the chatbot. You can obtain the bot ID using the method [imbot.bot.list](../bots/imbot-bot-list.md).

If this parameter is not provided, the method looks for the first bot registered by the current application ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified when registering the chatbot ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"CHAT_ID":2725,"TITLE":"New name for the chat","CLIENT_ID":"**put_your_client_id_here**"}' /
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.updateTitle
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"CHAT_ID":2725,"TITLE":"New name for the chat","auth":"**put_access_token_here**"}' /
    https://**put_your_bitrix24_address**/rest/imbot.chat.updateTitle
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.chat.updateTitle',
            {
                CHAT_ID: 2725,
                TITLE: 'New name for the chat',
            }
        );
        
        const result = response.getData().result;
        console.log('Chat title updated:', result);
        
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
                'imbot.chat.updateTitle',
                [
                    'CHAT_ID' => 2725,
                    'TITLE' => 'New name for the chat'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating chat title: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.updateTitle',
        {
            CHAT_ID: 2725,
            TITLE: 'New name for the chat',
        },
        function (result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.chat.updateTitle',
        [
            'CHAT_ID' => 2725,
            'TITLE' => 'New name for the chat'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1771936871,
        "finish": 1771936871.911049,
        "duration": 0.9110488891601562,
        "processing": 0,
        "date_start": "2026-02-24T15:41:11+01:00",
        "date_finish": "2026-02-24T15:41:11+01:00",
        "operating_reset_at": 1771937471,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the title was updated ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "TITLE_EMPTY",
    "error_description": "Title can't be empty"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided ||
|| `TITLE_EMPTY` | Title can't be empty | `TITLE` not provided ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat ||
|| `ACCESS_ERROR` | This chat cannot be renamed | This chat cannot be renamed ||
|| `BOT_ID_ERROR` | Bot not found | Chatbot not found ||
|| `APP_ID_ERROR` | Bot was installed by another rest application | Chatbot installed by another application ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Create a chat on behalf of the chat bot imbot.chat.add](./imbot-chat-add.md)
- [Add Participants to Chat imbot.chat.user.add](./imbot-chat-user-add.md)
- [Assign or Revoke Chat Administrator Rights imbot.chat.setManager](./imbot-chat-set-manager.md)
- [Change Chat Avatar imbot.chat.updateAvatar](./imbot-chat-update-avatar.md)
- [Change Chat Color imbot.chat.updateColor](./imbot-chat-update-color.md)
- [Get Chat ID imbot.chat.get](./imbot-chat-get.md)
- [Get Data About Chat imbot.dialog.get](./imbot-dialog-get.md)
- [Get the List of Chat Participants imbot.chat.user.list](./imbot-chat-user-list.md)
- [Exclude Participants from Chat imbot.chat.user.delete](./imbot-chat-user-delete.md)
- [Chat Bot Exit from Specified Chat imbot.chat.leave](./imbot-chat-leave.md)

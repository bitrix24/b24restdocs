# Create a chat on behalf of the chat bot imbot.chat.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: a user of the application that registered the chat bot

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Chat.add](../../chat-bots-v2/imbot.v2/chats/chat-add.md).

{% endnote %}

The method `imbot.chat.add` creates a chat on behalf of the chat bot.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE**
[`string`](../../../data-types.md) | Type of chat. Possible values:
- `OPEN` — open chat
- `CHAT` — closed chat

Default is `CHAT` ||
|| **TITLE**
[`string`](../../../data-types.md) | Title of the chat ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description of the chat ||
|| **COLOR**
[`string`](../../../data-types.md) | Color of the chat for the mobile application. Possible values:
- `RED` — red
- `GREEN` — green
- `MINT` — mint
- `LIGHT_BLUE` — light blue
- `DARK_BLUE` — dark blue
- `PURPLE` — purple
- `AQUA` — aqua
- `PINK` — pink
- `LIME` — lime
- `BROWN` — brown
- `AZURE` — azure
- `KHAKI` — khaki
- `SAND` — sand
- `MARENGO` — marengo
- `GRAY` — gray
- `GRAPHITE` — graphite ||
|| **MESSAGE**
[`string`](../../../data-types.md) | Welcome message in the chat ||
|| **USERS**
[`array`](../../../data-types.md) | Array of chat participants ||
|| **AVATAR**
[`string`](../../../data-types.md) | Avatar of the chat in [Base64](../../../files/how-to-upload-files.md) format.

Maximum image size is 5000x5000 ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | Type of the object to bind the chat to an external context ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Identifier of the object within `ENTITY_TYPE`. 

When creating a chat, you can pass any pair of `ENTITY_TYPE` and `ENTITY_ID`. The parameters are used to obtain the chat identifier using the method [imbot.chat.get](./imbot-chat-get.md) and to determine the context in event handlers [ONIMBOTMESSAGEADD](../messages/events/on-imbot-message-add.md), [ONIMBOTMESSAGEUPDATE](../messages/events/on-imbot-message-update.md), [ONIMBOTMESSAGEDELETE](../messages/events/on-imbot-message-delete.md) ||
|| **BOT_ID**
[`integer`](../../../data-types.md) | Identifier of the chat bot. You can get the bot identifier using the method [imbot.bot.list](../bots/imbot-bot-list.md).

If the parameter is not provided, the method searches for the first bot registered by the current application ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified when registering the chat bot ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"TYPE":"CHAT","TITLE":"New Chat","DESCRIPTION":"Important News","COLOR":"GREEN","MESSAGE":"Welcome!","USERS":[1271],"AVATAR":"/9j/4AAQSkZJRgABAQEBLAEsAAD/4QBwRXhp...+gKlSv+1v/2Q==","ENTITY_TYPE":"CHAT","ENTITY_ID":"13","BOT_ID":1291,"CLIENT_ID":"**put_your_client_id_here**"}' /
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"TYPE":"CHAT","TITLE":"New Chat","DESCRIPTION":"Important News","COLOR":"GREEN","MESSAGE":"Welcome!","USERS":[1271],"AVATAR":"/9j/4AAQSkZJRgABAQEBLAEsAAD/4QBwRXhp...+gKlSv+1v/2Q==","ENTITY_TYPE":"CHAT","ENTITY_ID":"13","BOT_ID":1291,"auth":"**put_access_token_here**"}' /
    https://**put_your_bitrix24_address**/rest/imbot.chat.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.chat.add',
            {
                TYPE: 'CHAT',
                TITLE: 'New Chat',
                DESCRIPTION: 'Important News',
                COLOR: 'GREEN',
                MESSAGE: 'Welcome!',
                USERS: [1271],
                AVATAR: '/9j/4AAQSkZJRgABAQEBLAEsAAD/4QBwRXhp...+gKlSv+1v/2Q==',
                ENTITY_TYPE: 'CHAT',
                ENTITY_ID: '13',
                BOT_ID: 1291
            }
        );
        
        const result = response.getData().result;
        console.log('Created chat with ID:', result);
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
                'imbot.chat.add',
                [
                    'TYPE' => 'CHAT',
                    'TITLE' => 'New Chat',
                    'DESCRIPTION' => 'Important News',
                    'COLOR' => 'GREEN',
                    'MESSAGE' => 'Welcome!',
                    'USERS' => [1271],
                    'AVATAR' => '/9j/4AAQSkZJRgABAQEBLAEsAAD/4QBwRXhp...+gKlSv+1v/2Q==',
                    'ENTITY_TYPE' => 'CHAT',
                    'ENTITY_ID' => '13',
                    'BOT_ID' => 1291
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding chat: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.add',
        {
            TYPE: 'CHAT',
            TITLE: 'New Chat',
            DESCRIPTION: 'Important News',
            COLOR: 'GREEN',
            MESSAGE: 'Welcome!',
            USERS: [1271],
            AVATAR: '/9j/4AAQSkZJRgABAQEBLAEsAAD/4QBwRXhp...+gKlSv+1v/2Q==',
            ENTITY_TYPE: 'CHAT',
            ENTITY_ID: '13',
            BOT_ID: 1291
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
        'imbot.chat.add',
        [
            'TYPE' => 'CHAT',
            'TITLE' => 'New Chat',
            'DESCRIPTION' => 'Important News',
            'COLOR' => 'GREEN',
            'MESSAGE' => 'Welcome!',
            'USERS' => [1271],
            'AVATAR' => '/9j/4AAQSkZJRgABAQEBLAEsAAD/4QBwRXhp...+gKlSv+1v/2Q==',
            'ENTITY_TYPE' => 'CHAT',
            'ENTITY_ID' => '13',
            'BOT_ID' => 1291
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
  "result": 2725,
  "time": {
      "start": 1771928379,
      "finish": 1771928380.102187,
      "duration": 1.102186918258667,
      "processing": 1,
      "date_start": "2026-02-24T13:19:39+01:00",
      "date_finish": "2026-02-24T13:19:40+01:00",
      "operating_reset_at": 1771928979,
      "operating": 0.3226499557495117
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created chat ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "WRONG_REQUEST",
    "error_description": "Chat can't be created"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `INVALID_FORMAT` | Parameter USERS has wrong type | The `USERS` parameter is in the wrong format ||
|| `BOT_ID_ERROR` | Bot not found | Chat bot not found ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | The specified chat bot was installed by another application ||
|| `WRONG_REQUEST` | Chat can't be created | Failed to create chat ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Add Participants to Chat imbot.chat.user.add](./imbot-chat-user-add.md)
- [Assign or Revoke Chat Administrator Rights imbot.chat.setManager](./imbot-chat-set-manager.md)
- [Change Chat Title imbot.chat.updateTitle](./imbot-chat-update-title.md)
- [Change Chat Avatar imbot.chat.updateAvatar](./imbot-chat-update-avatar.md)
- [Change Chat Color imbot.chat.updateColor](./imbot-chat-update-color.md)
- [Get Chat ID imbot.chat.get](./imbot-chat-get.md)
- [Get Data About Chat imbot.dialog.get](./imbot-dialog-get.md)
- [Get the List of Chat Participants imbot.chat.user.list](./imbot-chat-user-list.md)
- [Exclude Participants from Chat imbot.chat.user.delete](./imbot-chat-user-delete.md)
- [Chat Bot Exit from Specified Chat imbot.chat.leave](./imbot-chat-leave.md)

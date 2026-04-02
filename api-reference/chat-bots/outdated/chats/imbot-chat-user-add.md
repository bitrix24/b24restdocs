# Add Participants to Chat imbot.chat.user.add

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chat bot.

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Chat.User.add](../../chat-bots-v2/imbot.v2/chats/chat-user-add.md).

{% endnote %}

The method `imbot.chat.user.add` adds users to a chat.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the chat.

The identifier can be obtained using the method [imbot.chat.get](./imbot-chat-get.md) ||
|| **USERS***
[`array`](../../../data-types.md) | Array of user identifiers to be added.

User identifiers can be obtained using the methods [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) ||
|| **HIDE_HISTORY**
[`string`](../../../data-types.md) | Hide chat history for added users:
- `Y` — hide
- `N` — do not hide 

Default is `Y` ||
|| **BOT_ID**
[`integer`](../../../data-types.md) | Identifier of the chat bot. You can obtain the bot's identifier using the method [imbot.bot.list](../bots/imbot-bot-list.md).

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
    -d '{"CHAT_ID":2725,"USERS":[1269],"HIDE_HISTORY":"Y","CLIENT_ID":"**put_your_client_id_here**"}' /
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.user.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"CHAT_ID":2725,"USERS":[1269],"HIDE_HISTORY":"Y","auth":"**put_access_token_here**"}' /
    https://**put_your_bitrix24_address**/rest/imbot.chat.user.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.chat.user.add',
            {
                CHAT_ID: 2725,
                USERS: [1269],
                HIDE_HISTORY: 'Y'
            }
        );
        
        const result = response.getData().result;
        console.log('User added to chat:', result);
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
                'imbot.chat.user.add',
                [
                    'CHAT_ID' => 2725,
                    'USERS' => [1269],
                    'HIDE_HISTORY' => 'Y'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding user to chat: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.user.add',
        {
            CHAT_ID: 2725,
            USERS: [1269],
            HIDE_HISTORY: 'Y'
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
        'imbot.chat.user.add',
        [
            'CHAT_ID' => 2725,
            'USERS' => [1269],
            'HIDE_HISTORY' => 'Y'
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
    "result": true,
    "time": {
        "start": 1771935849,
        "finish": 1771935849.832795,
        "duration": 0.8327949047088623,
        "processing": 0,
        "date_start": "2026-02-24T15:24:09+01:00",
        "date_finish": "2026-02-24T15:24:09+01:00",
        "operating_reset_at": 1771936449,
        "operating": 0.14426183700561523
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if users were added ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` was not provided ||
|| `ACCESS_ERROR` | Action unavailable | Operation is not available for this chat ||
|| `ACCESS_ERROR` | It is forbidden to add users to this chat | Users cannot be added to this chat ||
|| `WRONG_REQUEST` | User IDs must be passed in array format | `USERS` was passed in an incorrect format ||
|| `WRONG_REQUEST` | You don't have access or user already member in chat | No permission to add or user is already in the chat ||
|| `BOT_ID_ERROR` | Bot not found | Chat bot not found ||
|| `APP_ID_ERROR` | Bot was installed by another rest application | Chat bot was installed by another application ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Create a chat on behalf of the chat bot imbot.chat.add](./imbot-chat-add.md)
- [Assign or Revoke Chat Administrator Rights imbot.chat.setManager](./imbot-chat-set-manager.md)
- [Change Chat Title imbot.chat.updateTitle](./imbot-chat-update-title.md)
- [Change Chat Avatar imbot.chat.updateAvatar](./imbot-chat-update-avatar.md)
- [Change Chat Color imbot.chat.updateColor](./imbot-chat-update-color.md)
- [Get Chat ID imbot.chat.get](./imbot-chat-get.md)
- [Get Data About Chat imbot.dialog.get](./imbot-dialog-get.md)
- [Get the List of Chat Participants imbot.chat.user.list](./imbot-chat-user-list.md)
- [Exclude Participants from Chat imbot.chat.user.delete](./imbot-chat-user-delete.md)
- [Chat Bot Exit from Specified Chat imbot.chat.leave](./imbot-chat-leave.md)

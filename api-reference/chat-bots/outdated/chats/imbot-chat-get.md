# Get Chat ID imbot.chat.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: user with access to the chat

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Chat.get](../../chat-bots-v2/imbot.v2/chats/chat-get.md).

{% endnote %}

The method `imbot.chat.get` returns the chat ID.

{% note info "" %}

To obtain detailed information about the chat, use the method [imbot.dialog.get](./imbot-dialog-get.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE***
[`string`](../../../data-types.md) | Type of the external object.

Used together with `ENTITY_ID`, which was passed when creating the chat using the method [imbot.chat.add](./imbot-chat-add.md) ||
|| **ENTITY_ID***
[`string`](../../../data-types.md) | Identifier of the external object.

Used together with `ENTITY_TYPE`, which was passed when creating the chat using the method [imbot.chat.add](./imbot-chat-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"ENTITY_TYPE":"CHAT","ENTITY_ID":"13"}' /
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"ENTITY_TYPE":"CHAT","ENTITY_ID":"13","auth":"**put_access_token_here**"}' /
    https://**put_your_bitrix24_address**/rest/imbot.chat.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.chat.get',
            {
                ENTITY_TYPE: 'CHAT',
                ENTITY_ID: '13'
            }
        );
        
        const result = response.getData().result;
        console.log('Retrieved chat data:', result);
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
                'imbot.chat.get',
                [
                    'ENTITY_TYPE' => 'CHAT',
                    'ENTITY_ID' => '13'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving chat: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.get',
        {
            ENTITY_TYPE: 'CHAT',
            ENTITY_ID: '13'
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
        'imbot.chat.get',
        [
            'ENTITY_TYPE' => 'CHAT',
            'ENTITY_ID' => '13'
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
  "result": {
      "ID": 2725
  },
  "time": {
      "start": 1771929873,
      "finish": 1771929873.459722,
      "duration": 0.45972204208374023,
      "processing": 0,
      "date_start": "2026-02-24T13:44:33+01:00",
      "date_finish": "2026-02-24T13:44:33+01:00",
      "operating_reset_at": 1771930473,
      "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Chat identifier.

Returns `null` if either `ENTITY_TYPE` or `ENTITY_ID` parameter is not provided ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to this chat"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `AUTHORIZE_ERROR` | Method not available for guest session. | Method is not available in guest session ||
|| `ACCESS_ERROR` | You don't have access to this chat | No access to the specified chat ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Create a chat on behalf of the chat bot imbot.chat.add](./imbot-chat-add.md)
- [Add Participants to Chat imbot.chat.user.add](./imbot-chat-user-add.md)
- [Assign or Revoke Chat Administrator Rights imbot.chat.setManager](./imbot-chat-set-manager.md)
- [Change Chat Title imbot.chat.updateTitle](./imbot-chat-update-title.md)
- [Change Chat Avatar imbot.chat.updateAvatar](./imbot-chat-update-avatar.md)
- [Change Chat Color imbot.chat.updateColor](./imbot-chat-update-color.md)
- [Get Data About Chat imbot.dialog.get](./imbot-dialog-get.md)
- [Get the List of Chat Participants imbot.chat.user.list](./imbot-chat-user-list.md)
- [Exclude Participants from Chat imbot.chat.user.delete](./imbot-chat-user-delete.md)
- [Chat Bot Exit from Specified Chat imbot.chat.leave](./imbot-chat-leave.md)

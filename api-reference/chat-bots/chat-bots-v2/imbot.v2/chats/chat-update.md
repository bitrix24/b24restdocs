# Update Chat Properties imbot.v2.Chat.update

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.update` updates the properties of a chat. It combines the update of the title, description, color, and avatar in a single call.

## Method Parameters

{% include [Parameter Note](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the registration of the chat bot ||
|| **dialogId***
[`string`](../../../../data-types.md) | Dialog ID. For group chats — `chat{chatId}`, for personal chats — `{userId}` ||
|| **fields***
[`object`](../../../../data-types.md) | Properties of the chat to be updated. The structure of the object is described [below](#fields) ||
|#

### Fields Parameter {#fields}

#|
|| **Name**
`Type` | **Description** ||
|| **title**
[`string`](../../../../data-types.md) | New title of the chat ||
|| **description**
[`string`](../../../../data-types.md) | New description of the chat ||
|| **color**
[`string`](../../../../data-types.md) | Chat color — [available colors](#available-colors) ||
|| **avatar**
[`file`](../../../../data-types.md) | New chat avatar in [Base64](../../../../files/how-to-upload-files.md) format ||
|#

### Available Colors {#available-colors}

#|
|| **Code** | **HEX** ||
|| `red` | `#df532d` ||
|| `green` | `#64a513` ||
|| `mint` | `#4ba984` ||
|| `lightBlue` | `#4ba5c3` ||
|| `darkBlue` | `#3e99ce` ||
|| `purple` | `#8474c8` ||
|| `aqua` | `#1eb4aa` ||
|| `pink` | `#f76187` ||
|| `lime` | `#58cc47` ||
|| `brown` | `#ab7761` ||
|| `azure` | `#29619b` ||
|| `khaki` | `#728f7a` ||
|| `sand` | `#ba9c7b` ||
|| `marengo` | `#556574` ||
|| `gray` | `#909090` ||
|| `graphite` | `#5e5f5e` ||
|#

## Code Examples

{% include [Example Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat5","fields":{"title":"New Title","color":"azure"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat5","fields":{"title":"New Title","color":"azure"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.update
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.update', {
        botId: 456,
        dialogId: 'chat5',
        fields: {
          title: 'New Title',
          color: 'azure',
        },
      });

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.v2.Chat.update',
                [
                    'botId' => 456,
                    'dialogId' => 'chat5',
                    'fields' => [
                        'title' => 'New Title',
                        'color' => 'azure',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: ' . print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.update',
        {
            botId: 456,
            dialogId: 'chat5',
            fields: {
                title: 'New Title',
                color: 'azure',
            },
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
        'imbot.v2.Chat.update',
        [
            'botId' => 456,
            'dialogId' => 'chat5',
            'fields' => [
                'title' => 'New Title',
                'color' => 'azure',
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Updated';
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
        "date_finish": "2024-10-11T10:00:00+02:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | `true` if the update was successful ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is not provided. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` is not provided ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot is registered by another application ||
|| `ACCESS_DENIED` | Access denied | Bot is not a participant in the chat ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Create Group Chat imbot.v2.Chat.add](./chat-add.md)
- [Get Information About the Chat imbot.v2.Chat.get](./chat-get.md)
- [Add Participants to Chat imbot.v2.Chat.User.add](./chat-user-add.md)
- [Assign Chat Owner imbot.v2.Chat.setOwner](./chat-set-owner.md)
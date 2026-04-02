# Get Information About the Bot imbot.v2.Bot.get

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: authorized user

The method `imbot.v2.Bot.get` returns information about the bot. It is used to verify the bot's installation.

For the owner application, it returns an extended format (including `moduleId`, `appId`, `eventMode`, and counters). For other applications, it provides a brief format.

## Method Parameters

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId**
[`integer`](../../../../data-types.md) | Bot ID. Required if `code` is not specified ||
|| **code**
[`string`](../../../../data-types.md) | Bot code. Required if `botId` is not specified ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for authorization via webhook, not needed for OAuth.

Pass the same botToken that was specified during the chat-bot registration ||
|#

{% note info "" %}

You must provide one of the parameters: `botId` or `code`.

{% endnote %}

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botToken":"my_bot_token","code":"support_bot"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Bot.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"code":"support_bot","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Bot.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Bot.get', {
        code: 'support_bot',
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
                'imbot.v2.Bot.get',
                [
                    'code' => 'support_bot',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: '. print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: '. $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Bot.get',
        {
            code: 'support_bot',
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
        'imbot.v2.Bot.get',
        ['code' => 'support_bot']
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'Bot ID: '. $result['result']['bot']['id'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "bot": {
            "id": 456,
            "code": "support_bot",
            "type": "bot",
            "isHidden": false,
            "isSupportOpenline": false,
            "isReactionsEnabled": true,
            "backgroundId": null,
            "language": "de",
            "moduleId": "rest",
            "appId": "local.67890abcdef12.34567890",
            "eventMode": "fetch",
            "countMessage": 150,
            "countCommand": 3,
            "countChat": 12,
            "countUser": 45
        },
        "users": [
            {
                "id": 456,
                "active": true,
                "name": "Support Bot",
                "bot": true,
                "type": "bot"
            }
        ]
    },
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
[`object`](../../../../data-types.md) | Result of the request ||
|| **result.bot**
[`Bot`](../../entities.md#bot) | Bot object. Extended format for the owner, brief format for others [(detailed description)](#bot-object) ||
|| **result.users**
[`User[]`](../../entities.md#user) | Array of related users [(detailed description)](#user-object) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the Bot Object {#bot-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Bot identifier ||
|| **code**
[`string`](../../../../data-types.md) | Symbolic code of the bot ||
|| **type**
[`string`](../../../../data-types.md) | Type of the bot ||
|| **isHidden**
[`boolean`](../../../../data-types.md) | Bot is hidden from the contact list ||
|| **isSupportOpenline**
[`boolean`](../../../../data-types.md) | Bot supports open channels ||
|| **isReactionsEnabled**
[`boolean`](../../../../data-types.md) | Reactions are enabled for bot messages ||
|| **backgroundId**
[`integer`](../../../../data-types.md) | Chat background identifier ||
|| **language**
[`string`](../../../../data-types.md) | Language of the bot ||
|| **moduleId**
[`string`](../../../../data-types.md) | Module identifier ||
|| **appId**
[`integer`](../../../../data-types.md) | Application identifier or `0` if the bot is not linked to an application ||
|| **eventMode**
[`string`](../../../../data-types.md) | Event delivery mode: `webhook` or `fetch` ||
|| **countMessage**
[`integer`](../../../../data-types.md) | Number of messages sent by the bot ||
|| **countCommand**
[`integer`](../../../../data-types.md) | Number of registered commands ||
|| **countChat**
[`integer`](../../../../data-types.md) | Number of bot chats ||
|| **countUser**
[`integer`](../../../../data-types.md) | Number of users who interacted with the bot ||
|#

### Fields of the User Object {#user-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | User identifier ||
|| **active**
[`boolean`](../../../../data-types.md) | User is active ||
|| **name**
[`string`](../../../../data-types.md) | User's first and last name ||
|| **bot**
[`boolean`](../../../../data-types.md) | Indicates if the user is a bot ||
|| **type**
[`string`](../../../../data-types.md) | Type of user ||
|#

Complete descriptions of all object fields can be found on the [Objects and Fields](../../entities.md) page.

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "BOT_NOT_FOUND",
    "error_description": "Bot not found"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is not provided. Required for authorization via webhook ||
|| `PARAMS_REQUIRED` | Required parameters are missing | Neither `botId` nor `code` was provided ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Register a Bot imbot.v2.Bot.register](./bot-register.md)
- [Update the bot imbot.v2.Bot.update](./bot-update.md)
- [List of Bots for the imbot.v2.Bot.list Application](./bot-list.md)
- [Unregister the bot imbot.v2.Bot.unregister](./bot-unregister.md)
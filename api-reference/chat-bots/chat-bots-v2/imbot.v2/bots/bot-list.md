# List of Bots for the imbot.v2.Bot.list Application

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Bot.list` returns a list of bots for the current application in an extended format.

## Method Parameters

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **filter**
[`object`](../../../../data-types.md) | Filter for results.

Available filter fields:
- `type` — type of the bot. Description of types — [Bot Types](../../index.md#bot-types) ||
|| **limit**
[`integer`](../../../../data-types.md) | Number of bots per page. Default is `50` ||
|| **offset**
[`integer`](../../../../data-types.md) | Offset for pagination. Default is `0` ||
|#

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botToken":"my_bot_token","filter":{"type":"bot"},"limit":10}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Bot.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"filter":{"type":"bot"},"limit":10,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Bot.list
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Bot.list', {
        filter: { type: 'bot' },
        limit: 10,
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
                'imbot.v2.Bot.list',
                [
                    'filter' => ['type' => 'bot'],
                    'limit' => 10,
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
        'imbot.v2.Bot.list',
        {
            filter: { type: 'bot' },
            limit: 10,
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
        'imbot.v2.Bot.list',
        [
            'filter' => ['type' => 'bot'],
            'limit' => 10,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        foreach ($result['result']['bots'] as $bot) {
            echo $bot['id']. ': '. $bot['code']. "\n";
        }
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "bots": [
            {
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
            }
        ],
        "users": [
            {
                "id": 456,
                "active": true,
                "name": "Support Bot",
                "bot": true,
                "type": "bot"
            }
        ],
        "hasNextPage": false
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
|| **result.bots**
[`Bot[]`](../../entities.md#bot) | Array of bots in extended format [(detailed description)](#bot-object) ||
|| **result.users**
[`User[]`](../../entities.md#user) | Array of related users [(detailed description)](#user-object) ||
|| **result.hasNextPage**
[`boolean`](../../../../data-types.md) | Is there a next page ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the Bot Object {#bot-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the bot ||
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
[`string|null`](../../../../data-types.md) | Chat background ID or `null` ||
|| **language**
[`string`](../../../../data-types.md) | Language of the bot ||
|| **moduleId**
[`string`](../../../../data-types.md) | Module identifier ||
|| **appId**
[`string`](../../../../data-types.md) | ID of the application that registered the bot ||
|| **eventMode**
[`string`](../../../../data-types.md) | Event delivery mode: `webhook` or `fetch` ||
|| **countMessage**
[`integer`](../../../../data-types.md) | Number of messages sent by the bot ||
|| **countCommand**
[`integer`](../../../../data-types.md) | Number of registered commands ||
|| **countChat**
[`integer`](../../../../data-types.md) | Number of chats for the bot ||
|| **countUser**
[`integer`](../../../../data-types.md) | Number of users who interacted with the bot ||
|#

### Fields of the User Object {#user-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the user ||
|| **active**
[`boolean`](../../../../data-types.md) | User is active ||
|| **name**
[`string`](../../../../data-types.md) | User's first and last name ||
|| **bot**
[`boolean`](../../../../data-types.md) | Indicates if the user is a bot ||
|| **type**
[`string`](../../../../data-types.md) | Type of user ||
|#

Complete description of all object fields is available on the [Objects and Fields](../../entities.md) page.

{% note info "" %}

The method always returns a successful response. If the application has no bots or an unknown value for `filter.type` is passed, an empty array `bots` is returned.

{% endnote %}

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "BOT_TOKEN_NOT_SPECIFIED",
    "error_description": "Bot token is not specified"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is not specified. Required for webhook authorization ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](./bot-register.md)
- [{#T}](./bot-get.md)
- [{#T}](./bot-update.md)
- [{#T}](./bot-unregister.md)
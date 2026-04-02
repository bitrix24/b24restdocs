# Register a Bot imbot.v2.Bot.register

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: authorized user

The method `imbot.v2.Bot.register` registers a new chat bot.

The method is idempotent: a repeated call with the same `fields.code` from the same application returns the existing bot without updating the data. To update, use [imbot.v2.Bot.update](./bot-update.md).

## Method Parameters

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **fields***
[`object`](../../../../data-types.md) | An object containing the bot parameters. Parameter descriptions are provided [below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`Type` | **Description** ||
|| **code***
[`string`](../../../../data-types.md) | Unique code for the bot within the application ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for authorization via webhook, not needed for OAuth.

Provide a unique botToken — this key will be tied to the chat bot and will be required for all subsequent calls to imbot.v2* via webhook ||
|| **properties***
[`object`](../../../../data-types.md) | Properties of the bot's profile. Parameter descriptions are provided [below](#properties) ||
|| **type**
[`string`](../../../../data-types.md) | Type of the bot. Descriptions of types and their behaviors can be found in [Bot Types](../../index.md#bot-types).

Default value: `bot` ||
|| **eventMode**
[`string`](../../../../data-types.md) | Event delivery mode.

Allowed values:
- `fetch` — the bot retrieves events via [imbot.v2.Event.get](../events/event-get.md)
- `webhook` — events are sent via POST request to `webhookUrl`

Default value: `fetch` ||
|| **webhookUrl**
[`string`](../../../../data-types.md) | URL of the bot's event handler. Required when `eventMode = webhook` ||
|| **isHidden**
[`boolean`](../../../../data-types.md) | Hidden bot. Allowed values: `true`, `false`. Default is `false` ||
|| **isReactionsEnabled**
[`boolean`](../../../../data-types.md) | Enable support for reactions. Allowed values: `true`, `false`. Default is `true` ||
|| **isSupportOpenline**
[`boolean`](../../../../data-types.md) | Enable support for Open Channels. Allowed values: `true`, `false`. Default is `false` ||
|| **backgroundId**
[`string`](../../../../data-types.md) | Background of the bot's chat. Default is `null` — uses the background from user settings. Available values are in the [backgrounds table](#backgrounds) ||
|#

### Parameter properties {#properties}

#|
|| **Name**
`Type` | **Description** ||
|| **name***
[`string`](../../../../data-types.md) | Name of the bot. Displayed in the chat list and dialog header ||
|| **lastName**
[`string`](../../../../data-types.md) | Last name of the bot ||
|| **workPosition**
[`string`](../../../../data-types.md) | Position of the bot (displayed in the profile) ||
|| **color**
[`string`](../../../../data-types.md) | Avatar color, [available colors](../chats/chat-add.md#available-colors). 
If not specified, it is assigned automatically ||
|| **gender**
[`string`](../../../../data-types.md) | Gender. Allowed values: `M`, `F` ||
|| **avatar**
[`file`](../../../../data-types.md) | Avatar. Provide a Base64 string without the prefix `data:*/*;base64,`.

How to prepare the data: [How to upload files](../../../../files/how-to-upload-files.md#how-to-encode-a-file-in-base64) ||
|#

### Available Backgrounds {#backgrounds}

#|
|| **ID** | **Theme** | **Color** ||
|| `azure` | dark | Blue ||
|| `mint` | dark | Mint ||
|| `steel` | dark | Steel ||
|| `slate` | dark | Slate ||
|| `teal` | dark | Teal ||
|| `cornflower` | dark | Cornflower ||
|| `sky` | light | Sky ||
|| `peach` | light | Peach ||
|| `frost` | light | Frost ||
|#

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "fields": {
          "code": "support_bot",
          "botToken": "my_bot_token",
          "properties": {"name": "Support Bot", "workPosition": "AI Assistant"},
          "type": "bot",
          "eventMode": "fetch"
        }
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Bot.register
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "fields": {
          "code": "support_bot",
          "properties": {"name": "Support Bot", "workPosition": "AI Assistant"},
          "type": "bot",
          "eventMode": "fetch"
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Bot.register
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Bot.register', {
        fields: {
          code: 'support_bot',
          properties: {
            name: 'Support Bot',
            workPosition: 'AI Assistant',
          },
          type: 'bot',
          eventMode: 'fetch',
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
                'imbot.v2.Bot.register',
                [
                    'fields' => [
                        'code' => 'support_bot',
                        'properties' => [
                            'name' => 'Support Bot',
                            'workPosition' => 'AI Assistant',
                        ],
                        'type' => 'bot',
                        'eventMode' => 'fetch',
                    ],
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
        'imbot.v2.Bot.register',
        {
            fields: {
                code: 'support_bot',
                properties: {
                    name: 'Support Bot',
                    workPosition: 'AI Assistant',
                },
                type: 'bot',
                eventMode: 'fetch',
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
        'imbot.v2.Bot.register',
        [
            'fields' => [
                'code' => 'support_bot',
                'properties' => [
                    'name' => 'Support Bot',
                    'workPosition' => 'AI Assistant',
                ],
                'type' => 'bot',
                'eventMode' => 'fetch',
            ],
        ]
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
            "language": "en",
            "moduleId": "rest",
            "appId": "local.67890abcdef12.34567890",
            "eventMode": "fetch",
            "countMessage": 0,
            "countCommand": 0,
            "countChat": 0,
            "countUser": 0
        },
        "users": [
            {
                "id": 456,
                "active": true,
                "name": "Support Bot",
                "firstName": "Support",
                "lastName": "Bot",
                "workPosition": "AI Assistant",
                "color": "#df532d",
                "avatar": "",
                "gender": "M",
                "birthday": "",
                "extranet": false,
                "bot": true,
                "connector": false,
                "externalAuthId": "bot",
                "status": "online",
                "idle": false,
                "lastActivityDate": "2025-01-15T10:30:00+01:00",
                "absent": false,
                "departments": [1],
                "phones": false,
                "type": "bot"
            }
        ]
    },
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+01:00",
        "date_finish": "2024-10-11T10:00:00+01:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Result of bot registration ||
|| **result.bot**
[`Bot`](../../entities.md#bot) | Bot object in extended format (available only to the owner) [(detailed description)](#bot-object) ||
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
[`integer`](../../../../data-types.md) | Identifier of the bot ||
|| **code**
[`string`](../../../../data-types.md) | Symbolic code of the bot ||
|| **type**
[`string`](../../../../data-types.md) | Type of the bot ||
|| **isHidden**
[`boolean`](../../../../data-types.md) | Bot is hidden from the contact list ||
|| **isSupportOpenline**
[`boolean`](../../../../data-types.md) | Bot supports open lines ||
|| **isReactionsEnabled**
[`boolean`](../../../../data-types.md) | Reactions are enabled for bot messages ||
|| **backgroundId**
[`integer`](../../../../data-types.md) | Identifier of the chat background ||
|| **language**
[`string`](../../../../data-types.md) | Language of the bot ||
|| **moduleId**
[`string`](../../../../data-types.md) | Identifier of the module ||
|| **appId**
[`integer`](../../../../data-types.md) | Identifier of the application or `0` if the bot is not tied to an application ||
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
[`string`](../../../../data-types.md) | User's full name ||
|| **firstName**
[`string`](../../../../data-types.md) | User's first name ||
|| **lastName**
[`string`](../../../../data-types.md) | User's last name ||
|| **workPosition**
[`string`](../../../../data-types.md) | Position ||
|| **color**
[`string`](../../../../data-types.md) | User's color ||
|| **avatar**
[`string`](../../../../data-types.md) | Avatar URL ||
|| **gender**
[`string`](../../../../data-types.md) | User's gender ||
|| **birthday**
[`string`](../../../../data-types.md) | Date of birth ||
|| **extranet**
[`boolean`](../../../../data-types.md) | User is an extranet user ||
|| **bot**
[`boolean`](../../../../data-types.md) | Indicates if the user is a bot ||
|| **connector**
[`boolean`](../../../../data-types.md) | User is connected via a connector ||
|| **externalAuthId**
[`string`](../../../../data-types.md) | External type of authorization ||
|| **status**
[`string`](../../../../data-types.md) | User's status ||
|| **idle**
[`boolean`](../../../../data-types.md) | User is inactive ||
|| **lastActivityDate**
[`string`](../../../../data-types.md) | Date of last activity ||
|| **absent**
[`boolean`](../../../../data-types.md) | User is absent ||
|| **departments**
[`array`](../../../../data-types.md) | List of departments ||
|| **phones**
[`array`](../../../../data-types.md) | List of phones ||
|| **type**
[`string`](../../../../data-types.md) | Type of user ||
|#

Complete descriptions of all object fields can be found on the [Objects and Fields](../../entities.md) page.

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "BOT_CODE_REQUIRED",
    "error_description": "Bot code is required"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `fields.botToken` is not specified. Required for authorization via webhook ||
|| `BOT_CODE_REQUIRED` | Bot code is required | Bot code (`fields.code`) is not specified ||
|| `BOT_PROPERTIES_REQUIRED` | Bot properties are required | Bot properties (name) are not specified ||
|| `BOT_CODE_ALREADY_TAKEN` | Bot code is already taken | The bot code is already taken by another application ||
|| `BOT_INVALID_TYPE` | Invalid bot type | Invalid bot type. Allowed values: `bot`, `network`, `openline`, `supervisor`, `personal` ||
|| `BOT_INVALID_EVENT_MODE` | Invalid event mode | Invalid event delivery mode. Allowed values: `fetch`, `webhook` ||
|| `BOT_WEBHOOK_URL_REQUIRED` | Webhook URL is required | `fields.webhookUrl` is not specified when `fields.eventMode = webhook` ||
|| `BOT_REGISTER_FAILED` | Bot registration failed | Error registering the bot ||
|| `BOT_INVALID_CALLBACK` | Invalid callback URL | Invalid handler URL ||
|| `BOT_LIMIT_EXCEEDED` | Bot limit exceeded | Application bot limit exceeded (100 bots) ||
|| `BOT_AVATAR_INCORRECT_TYPE` | Avatar must be an image | Avatar must be an image (`image/*`) ||
|| `BOT_AVATAR_INCORRECT_SIZE` | Avatar exceeds maximum size | Avatar size exceeds maximum (5000×5000 px) ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Update the bot imbot.v2.Bot.update](./bot-update.md)
- [Get Information About the Bot imbot.v2.Bot.get](./bot-get.md)
- [List of Bots for the imbot.v2.Bot.list Application](./bot-list.md)
- [Unregister the bot imbot.v2.Bot.unregister](./bot-unregister.md)
- [Get Bot Events imbot.v2.Event.get](../events/event-get.md)
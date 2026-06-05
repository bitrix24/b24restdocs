# Update the imbot.v2.Bot.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Bot.update` updates the properties of the bot.

## Method Parameters

{% include [Note on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **fields***
[`object`](../../../../data-types.md) | Fields to be updated. The structure of the object is described [below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`Type` | **Description** ||
|| **properties**
[`object`](../../../../data-types.md) | Properties of the bot's profile. Parameter descriptions are [below](#properties) ||
|| **isHidden**
[`boolean`](../../../../data-types.md) | Hidden bot. Acceptable values: `true`, `false` ||
|| **isReactionsEnabled**
[`boolean`](../../../../data-types.md) | Support for reactions. Acceptable values: `true`, `false` ||
|| **isSupportOpenline**
[`boolean`](../../../../data-types.md) | Support for Open Channels. Acceptable values: `true`, `false` ||
|| **backgroundId**
[`string`](../../../../data-types.md) | Background of the bot's chat. Pass `null` to reset to the user's background. Available values are in the [backgrounds table](./bot-register.md#backgrounds). An invalid value is normalized to `null` ||
|| **eventMode**
[`string`](../../../../data-types.md) | Event delivery mode: `fetch` or `webhook` ||
|| **webhookUrl**
[`string`](../../../../data-types.md) | URL of the event handler (applies when `eventMode = webhook`) ||
|| **botToken**
[`string`](../../../../data-types.md) | New `botToken` for token rotation. Request authorization is performed with the **old** token passed at the top level in the `botToken` parameter. An empty string or a string of spaces is ignored. Maximum length is 40 characters.

More details in the section [BotToken Rotation](#token-rotation) ||
|#

### Parameter properties {#properties}

#|
|| **Name**
`Type` | **Description** ||
|| **name**
[`string`](../../../../data-types.md) | Bot's name ||
|| **lastName**
[`string`](../../../../data-types.md) | Bot's last name ||
|| **workPosition**
[`string`](../../../../data-types.md) | Bot's position (displayed in the profile) ||
|| **color**
[`string`](../../../../data-types.md) | Avatar color, [available colors](../chats/chat-add.md#available-colors). 
If specified incorrectly, it is ignored ||
|| **gender**
[`string`](../../../../data-types.md) | Gender. Acceptable values: `M`, `F` ||
|| **avatar**
[`file`](../../../../data-types.md) | Avatar. Pass a Base64 string without the prefix `data:*/*;base64,`.

How to prepare data: [How to upload files](../../../../files/how-to-upload-files.md#how-to-encode-file-to-base64) ||
|#

## BotToken Rotation {#token-rotation}

Passing `fields.botToken` replaces the current bot token with a new one. Request authorization is performed with the **old** token at the top level; the new token is passed in `fields.botToken`.

After a successful rotation:

- All the bot's subscriptions to events `ONIMBOTV2*` are re-linked to the new `APPLICATION_TOKEN` if the bot operates in `webhook` mode.
- The old token **instantly** loses access to the bot — subsequent requests with it will return the error `BOT_OWNERSHIP_ERROR`.

In case of a collision (the new token is already linked to another bot), a generic error `BOT_TOKEN_ROTATION_FAILED` is returned without specifying the reason, to avoid revealing the existence of foreign tokens.

## Managing Event Subscriptions {#event-subscriptions}

When calling `Bot.update`, the bot's subscriptions to events `ONIMBOTV2*` are automatically brought up to date:

#|
|| **Change** | **Behavior** ||
|| `webhookUrl` changes (mode remains `webhook`) | The old eight subscriptions to the previous URL **are deleted**, and eight new ones are created for the new URL. The previous URL stops receiving events ||
|| `eventMode`: `webhook` → `fetch` | All eight subscriptions of the bot **are deleted**. After the transition, events are only available through [imbot.v2.Event.get](../events/event-get.md) ||
|| `eventMode`: `fetch` → `webhook` | Eight subscriptions are created for the specified `webhookUrl`. If there were any "legacy" subscriptions on another URL, they are also deleted before creating new ones ||
|| Other fields `properties`, `isHidden`, and others | Subscriptions remain unchanged ||
|#

{% note info "" %}

**OAuth applications with multiple bots:** if one application has registered multiple bots under a common `clientId`, the automatic cleanup of subscriptions when changing `webhookUrl` or `eventMode` is skipped to avoid affecting subscriptions of neighboring bots. Webhook-authorized bots with their own `botToken` do not have this limitation — each of them has its own synthetic `APPLICATION_TOKEN`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","fields":{"properties":{"name":"Updated Bot"},"isHidden":true}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Bot.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"fields":{"properties":{"name":"Updated Bot"},"isHidden":true},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Bot.update
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Bot.update', {
        botId: 456,
        fields: {
          properties: { name: 'Updated Bot' },
          isHidden: true,
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
                'imbot.v2.Bot.update',
                [
                    'botId' => 456,
                    'fields' => [
                        'properties' => ['name' => 'Updated Bot'],
                        'isHidden' => true,
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
        'imbot.v2.Bot.update',
        {
            botId: 456,
            fields: {
                properties: { name: 'Updated Bot' },
                isHidden: true,
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
        'imbot.v2.Bot.update',
        [
            'botId' => 456,
            'fields' => [
                'properties' => ['name' => 'Updated Bot'],
                'isHidden' => true,
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'Success';
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "bot": {
            "id": 456,
            "code": "support_bot",
            "type": "bot",
            "isHidden": true,
            "isSupportOpenline": false,
            "isReactionsEnabled": true,
            "backgroundId": null,
            "language": "de",
            "moduleId": "rest",
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
                "name": "Updated Bot",
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
[`object`](../../../../data-types.md) | Update result ||
|| **result.bot**
[`Bot`](../../entities.md#bot) | Updated bot object in extended format [(detailed description)](#bot-object) ||
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
[`string`](../../../../data-types.md) | Type of bot ||
|| **isHidden**
[`boolean`](../../../../data-types.md) | Bot is hidden from the contact list ||
|| **isSupportOpenline**
[`boolean`](../../../../data-types.md) | Bot supports open channels ||
|| **isReactionsEnabled**
[`boolean`](../../../../data-types.md) | Reactions are enabled for bot messages ||
|| **backgroundId**
[`string|null`](../../../../data-types.md) | Chat background ID or `null` ||
|| **language**
[`string`](../../../../data-types.md) | Bot's language ||
|| **moduleId**
[`string`](../../../../data-types.md) | Module identifier ||
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
[`boolean`](../../../../data-types.md) | Indicates a bot user ||
|| **type**
[`string`](../../../../data-types.md) | User type ||
|#

A complete description of all object fields can be found on the [Objects and Fields](../../entities.md) page.

## Error Handling

HTTP status: **400**, **403**

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
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is not specified. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` is not specified ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot is registered by another application ||
|| `BOT_INVALID_EVENT_MODE` | Invalid event mode | Invalid event delivery mode ||
|| `BOT_INVALID_CALLBACK` | Invalid callback URL | Invalid event handler URL ||
|| `BOT_AVATAR_INCORRECT_TYPE` | Avatar must be an image | Avatar must be an image (`image/*`) ||
|| `BOT_AVATAR_INCORRECT_SIZE` | Avatar exceeds maximum size | Avatar size exceeds maximum (5000×5000 px) ||
|| `BOT_TOKEN_ROTATION_FAILED` | Bot token rotation failed | Bot token rotation failed. Generic error code — reason is not disclosed to avoid confirming the existence of foreign tokens ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](./bot-register.md)
- [{#T}](./bot-get.md)
- [{#T}](./bot-list.md)
- [{#T}](./bot-unregister.md)
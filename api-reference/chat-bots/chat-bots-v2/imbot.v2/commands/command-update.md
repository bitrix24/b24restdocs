# Update the imbot.v2.Command.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Command.update` updates the bot's slash command.

## Method Parameters

{% include [Parameter Note](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **commandId***
[`integer`](../../../../data-types.md) | Command ID ||
|| **fields***
[`object`](../../../../data-types.md) | Fields of the command to be updated. The structure of the object is described [below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`Type` | **Description** ||
|| **command**
[`string`](../../../../data-types.md) | New command (without `/`) ||
|| **title**
[`object`](../../../../data-types.md) | Title of the command in different languages:

- string value updates the translation
- value `null` removes the translation
- absence of the key leaves the translation unchanged

Example: `{"de": "Neu", "en": null}` will update `de` and remove `en` ||
|| **params**
[`object`](../../../../data-types.md) | Description of parameters in different languages. Similar to `title`, applies only to those languages provided in `title` ||
|| **common**
[`string`](../../../../data-types.md) | Common command. Allowed values: `Y`, `N` ||
|| **hidden**
[`string`](../../../../data-types.md) | Hidden command. Allowed values: `Y`, `N` ||
|| **extranetSupport**
[`string`](../../../../data-types.md) | Extranet support. Allowed values: `Y`, `N` ||
|#

## Code Examples

{% include [Example Note](../../../../../_includes/examples.md) %}

### Example 1. Updating Translations

Update the title in `de` and `en`. Translations in other languages, if any, will remain unchanged.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"b15f6e80ef345c97e23db31e727281f4","commandId":42,"fields":{"title":{"en":"Updated help","de":"Aktualisierte Hilfe"}}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Command.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"commandId":42,"fields":{"title":{"en":"Updated help","de":"Aktualisierte Hilfe"}},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Command.update
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Command.update', {
        botId: 456,
        commandId: 42,
        fields: {
          title: {
            en: 'Updated help',
            de: 'Aktualisierte Hilfe',
          },
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
                'imbot.v2.Command.update',
                [
                    'botId' => 456,
                    'commandId' => 42,
                    'fields' => [
                        'title' => [
                            'en' => 'Updated help',
                            'de' => 'Aktualisierte Hilfe',
                        ],
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
        'imbot.v2.Command.update',
        {
            botId: 456,
            commandId: 42,
            fields: {
                title: {
                    en: 'Updated help',
                    de: 'Aktualisierte Hilfe',
                },
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
        'imbot.v2.Command.update',
        [
            'botId' => 456,
            'commandId' => 42,
            'fields' => [
                'title' => [
                    'en' => 'Updated help',
                    'de' => 'Aktualisierte Hilfe',
                ],
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'result: '. print_r($result['result'], true);
    }
    ```

{% endlist %}

### Example 2. Deleting a Translation

Update `de` and remove `en`. To delete, pass `null` as the value.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"b15f6e80ef345c97e23db31e727281f4","commandId":42,"fields":{"title":{"de":"Hilfe","en":null}}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Command.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"commandId":42,"fields":{"title":{"de":"Hilfe","en":null}},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Command.update
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Command.update', {
        botId: 456,
        commandId: 42,
        fields: {
          title: {
            de: 'Hilfe',
            en: null,
          },
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
                'imbot.v2.Command.update',
                [
                    'botId' => 456,
                    'commandId' => 42,
                    'fields' => [
                        'title' => [
                            'de' => 'Hilfe',
                            'en' => null,
                        ],
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
        'imbot.v2.Command.update',
        {
            botId: 456,
            commandId: 42,
            fields: {
                title: {
                    de: 'Hilfe',
                    en: null,
                },
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
        'imbot.v2.Command.update',
        [
            'botId' => 456,
            'commandId' => 42,
            'fields' => [
                'title' => [
                    'de' => 'Hilfe',
                    'en' => null,
                ],
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'result: '. print_r($result['result'], true);
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "command": {
            "id": 42,
            "botId": 456,
            "command": "/help",
            "common": false,
            "hidden": false,
            "extranetSupport": false
        }
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
[`object`](../../../../data-types.md) | Result of the operation ||
|| **result.command**
[`Command`](../../entities.md#command) | Object of the updated command [(detailed description)](#command-object) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the Command Object {#command-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the command ||
|| **botId**
[`integer`](../../../../data-types.md) | Identifier of the bot ||
|| **command**
[`string`](../../../../data-types.md) | Text of the command ||
|| **common**
[`boolean`](../../../../data-types.md) | Command available in all chats ||
|| **hidden**
[`boolean`](../../../../data-types.md) | Command hidden from the command list ||
|| **extranetSupport**
[`boolean`](../../../../data-types.md) | Command available to extranet users ||
|#

Complete description of all object fields can be found on the [Objects and Fields](../../entities.md) page.

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "COMMAND_NOT_FOUND",
    "error_description": "Command not found"
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
|| `COMMAND_NOT_FOUND` | Command not found | Command not found or no access ||
|| `COMMAND_NAME_EMPTY` | Command name is empty | An empty command name was provided ||
|| `COMMAND_NAME_INVALID` | Command name is invalid | Command name must be a string ||
|| `COMMAND_ALREADY_EXISTS` | Command already exists | A command with this name is already registered for this bot ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./command-register.md)
- [{#T}](./command-list.md)
- [{#T}](./command-unregister.md)
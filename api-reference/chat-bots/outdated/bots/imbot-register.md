# Create a Chat-Bot imbot.register

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registers the chat-bot.

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Bot.register](../../chat-bots-v2/imbot.v2/bots/bot-register.md).

{% endnote %}

The method `imbot.register` registers a chat-bot and binds event handlers of the application.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CODE***
[`string`](../../../data-types.md) | The string code of the bot, unique within Bitrix24 ||
|| **TYPE**
[`string`](../../../data-types.md) | The type of the bot.

Allowed values:
- `B` — standard chat-bot. In group chats, it sees messages that are specifically addressed to it.
- `O` — chat-bot for Open Channels.
- `H` — chat-bot in "human" mode. Before auto-replying, it activates the "typing..." status `startWriting`.
- `S` — chat-bot with elevated privileges (supervisor). It reads all messages in the chats it is part of. If the bot was added with history visibility, it sees both old and new messages. If added without history, it only sees new messages.

Default value: `B` ||
|| **OPENLINE**
[`string`](../../../data-types.md) | Operation mode with Open Channels.

Allowed values:
- `Y` — enable Open Channels support mode
- `N` — disable, default value

For `TYPE=O`, this parameter can be omitted. The value will be forcibly set to `Y` ||
|| **EVENT_HANDLER**
[`string`](../../../data-types.md) | General URL for the event handler. If provided, its value is copied to `EVENT_MESSAGE_ADD`, `EVENT_MESSAGE_UPDATE`, `EVENT_MESSAGE_DELETE`, `EVENT_WELCOME_MESSAGE`, `EVENT_BOT_DELETE`.

If different handlers are needed, do not pass `EVENT_HANDLER`. Specify separate URLs in the parameters `EVENT_MESSAGE_ADD`, `EVENT_MESSAGE_UPDATE`, `EVENT_MESSAGE_DELETE`, `EVENT_WELCOME_MESSAGE`, `EVENT_BOT_DELETE` ||
|| **EVENT_MESSAGE_ADD***
[`string`](../../../data-types.md) | URL for the event handler [ONIMBOTMESSAGEADD](../messages/events/on-imbot-message-add.md) ||
|| **EVENT_MESSAGE_UPDATE**
[`string`](../../../data-types.md) | URL for the event handler [ONIMBOTMESSAGEUPDATE](../messages/events/on-imbot-message-update.md).

This parameter is ignored only for bots with `TYPE=B/H` and `OPENLINE=N` ||
|| **EVENT_MESSAGE_DELETE**
[`string`](../../../data-types.md) | URL for the event handler [ONIMBOTMESSAGEDELETE](../messages/events/on-imbot-message-delete.md).

This parameter is ignored only for bots with `TYPE=B/H` and `OPENLINE=N` ||
|| **EVENT_WELCOME_MESSAGE***
[`string`](../../../data-types.md) | URL for the event handler [ONIMBOTJOINCHAT](../chats/events/on-imbot-join-chat.md) ||
|| **EVENT_BOT_DELETE***
[`string`](../../../data-types.md) | URL for the event handler [ONIMBOTDELETE](../events/on-imbot-delete.md) ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Provide a unique CLIENT_ID — this key will be linked to the chat-bot and will be needed for all subsequent calls to imbot* via webhook ||
|| **PROPERTIES***
[`object`](../../../data-types.md) | Properties of the chat-bot profile. The structure of the object is described in detail [below](#properties) ||
|#

### Parameter PROPERTIES {#properties}

#|
|| **Name**
`Type` | **Description** ||
|| **NAME***
[`string`](../../../data-types.md) | The name of the chat-bot. You must provide either `NAME` or `LAST_NAME` ||
|| **LAST_NAME***
[`string`](../../../data-types.md) | The last name of the chat-bot. You must provide either `NAME` or `LAST_NAME` ||
|| **COLOR**
[`string`](../../../data-types.md) | The color of the chat-bot for the mobile interface: `RED`, `GREEN`, `MINT`, `LIGHT_BLUE`, `DARK_BLUE`, `PURPLE`, `AQUA`, `PINK`, `LIME`, `BROWN`, `AZURE`, `KHAKI`, `SAND`, `MARENGO`, `GRAY`, `GRAPHITE` ||
|| **EMAIL**
[`string`](../../../data-types.md) | Email for contacting the chat-bot. The bot is created as a user, so the bot's email must not match the email of a real Bitrix24 user. This will help avoid account conflicts ||
|| **PERSONAL_BIRTHDAY**
[`string`](../../../data-types.md) | Birthday in the format `YYYY-MM-DD` ||
|| **WORK_POSITION**
[`string`](../../../data-types.md) | Position or description of the chat-bot ||
|| **PERSONAL_WWW**
[`string`](../../../data-types.md) | Website link ||
|| **PERSONAL_GENDER**
[`string`](../../../data-types.md) | Gender, allowed values: `M` or `F` ||
|| **PERSONAL_PHOTO**
[`file`](../../../data-types.md) | Avatar of the chat-bot in [Base64](../../../files/how-to-upload-files.md)

The image size must not exceed the limit of 5000x5000 ||
|#

{% note info "" %}

If the application logic allows, send a response to the bot upon explicit mention. This can be checked via the `TO_USER_ID` field.

{% endnote %}

{% note warning "" %}

Only one set of event handler URLs can be used in a single application.

If registering a second bot, the parameters `EVENT_MESSAGE_ADD`, `EVENT_WELCOME_MESSAGE`, and `EVENT_BOT_DELETE` must match those of the first bot.

If there are multiple bots in the application, differentiate them within a single event handler. Pass an array of bots in the event.

The maximum number of bots for a single application: `5`

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
      -H "Content-Type: application/json" /
      -H "Accept: application/json" /
      -d '{"CODE":"newbot","TYPE":"B","EVENT_HANDLER":"https://example.com/bot/events","OPENLINE":"N","PROPERTIES":{"NAME":"NewBot","WORK_POSITION":"Support bot"},"CLIENT_ID":"**put_your_client_id_here**"}' /
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.register
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
      -H "Content-Type: application/json" /
      -H "Accept: application/json" /
      -d '{"CODE":"newbot","TYPE":"B","EVENT_HANDLER":"https://example.com/bot/events","OPENLINE":"N","PROPERTIES":{"NAME":"NewBot"},"auth":"**put_access_token_here**"}' /
      https://**put_your_bitrix24_address**/rest/imbot.register
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.register', {
        CODE: 'newbot',
        TYPE: 'B',
        EVENT_HANDLER: 'https://example.com/bot/events',
        OPENLINE: 'N',
        PROPERTIES: {
          NAME: 'NewBot',
          WORK_POSITION: 'Support bot',
        },
      });

      const { result } = response.getData();
      console.log('Created bot ID:', result);
    } catch (error) {
      console.error('Error registering bot:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.register',
                [
                    'CODE' => 'newbot',
                    'TYPE' => 'B',
                    'EVENT_HANDLER' => 'https://example.com/bot/events',
                    'OPENLINE' => 'N',
                    'PROPERTIES' => [
                        'NAME' => 'NewBot',
                        'WORK_POSITION' => 'Support bot',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Created bot ID: ' . $result->data();
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error registering bot: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.register',
        {
            CODE: 'newbot',
            TYPE: 'B',
            EVENT_HANDLER: 'https://example.com/bot/events',
            OPENLINE: 'N',
            PROPERTIES: {
                NAME: 'NewBot',
                WORK_POSITION: 'Support bot',
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
        'imbot.register',
        [
            'CODE' => 'newbot',
            'TYPE' => 'B',
            'EVENT_HANDLER' => 'https://example.com/bot/events',
            'OPENLINE' => 'N',
            'PROPERTIES' => [
                'NAME' => 'NewBot',
                'WORK_POSITION' => 'Support bot',
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Created bot ID: ' . $result['result'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": 39,
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00",
        "operating_reset_at": 1762349466,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created chat-bot `BOT_ID` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "CODE_ERROR",
    "error_description": "Bot code isn't specified"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization | The method was called with session authorization instead of OAuth or webhook ||
|| `ACCESS_DENIED` | Access denied! Client ID not specified | Unable to determine the application: `clientId` authorization is missing and `CLIENT_ID` was not provided ||
|| `EVENT_MESSAGE_ADD_ERROR` | Handler for "Message add" event isn't specified | The required event handler `EVENT_MESSAGE_ADD` was not provided ||
|| `EVENT_MESSAGE_ADD_ERROR` | Wrong handler URL | An invalid URL was provided for `EVENT_MESSAGE_ADD` ||
|| `EVENT_MESSAGE_UPDATE_ERROR` | Wrong handler URL | An invalid URL was provided for `EVENT_MESSAGE_UPDATE` ||
|| `EVENT_MESSAGE_DELETE_ERROR` | Wrong handler URL | An invalid URL was provided for `EVENT_MESSAGE_DELETE` ||
|| `EVENT_WELCOME_MESSAGE_ERROR` | Handler for "Welcome message" event isn't specified | The required event handler `EVENT_WELCOME_MESSAGE` was not provided ||
|| `EVENT_WELCOME_MESSAGE_ERROR` | Wrong handler URL | An invalid URL was provided for `EVENT_WELCOME_MESSAGE` ||
|| `EVENT_BOT_DELETE_ERROR` | Handler for "Bot delete" event isn't specified | The required event handler `EVENT_BOT_DELETE` was not provided ||
|| `EVENT_BOT_DELETE_ERROR` | Wrong handler URL | An invalid URL was provided for `EVENT_BOT_DELETE` ||
|| `CODE_ERROR` | Bot code isn't specified | The required bot code `CODE` was not provided ||
|| `NAME_ERROR` | Bot name isn't specified | One of the required fields in `PROPERTIES` is missing: `NAME` or `LAST_NAME` ||
|| `MAX_COUNT_ERROR` | Has reached the maximum number of bots for application (max: N) | The limit on the number of bots for the application has been reached ||
|| `WRONG_REQUEST` | Bot can't be created | The bot cannot be created ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Update Chat-Bot imbot.update](./imbot-update.md)
- [Unregister Chat Bot imbot.unregister](./imbot-unregister.md)
- [Get the List of Chat Bots imbot.bot.list](./imbot-bot-list.md)
- [About Events](../events/index.md)

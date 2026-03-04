# Update Chatbot imbot.update

> Scope: [`imbot`](../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chatbot

The method `imbot.update` updates the chatbot's data and its event handlers.

## Method Parameters

{% include [Parameters Note](../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **BOT_ID***
[`integer`](../data-types.md) | The identifier of the chatbot. You can obtain the identifier using the [imbot.bot.list](./imbot-bot-list.md) method. The value must be greater than `0` ||
|| **FIELDS***
[`object`](../data-types.md) | Data for updating the chatbot. The structure of the object is described in detail [below](#fields) ||
|| **CLIENT_ID**
[`string`](../data-types.md) | A technical parameter for scenarios without `clientId` in authorization. If provided, it is used as `custom{CLIENT_ID}` to identify the application ||
|#

### FIELDS Parameter {#fields}

#|
|| **Name**
`Type` | **Description** ||
|| **CODE**
[`string`](../data-types.md) | The new string code for the bot, unique within Bitrix24  ||
|| **EVENT_HANDLER**
[`string`](../data-types.md) | General URL for the event handler. If provided, its value is copied to `EVENT_MESSAGE_ADD`, `EVENT_MESSAGE_UPDATE`, `EVENT_MESSAGE_DELETE`, `EVENT_WELCOME_MESSAGE`, `EVENT_BOT_DELETE`.

If different handlers are needed, do not provide `EVENT_HANDLER`. Set separate URLs in the parameters `EVENT_MESSAGE_ADD`, `EVENT_MESSAGE_UPDATE`, `EVENT_MESSAGE_DELETE`, `EVENT_WELCOME_MESSAGE`, `EVENT_BOT_DELETE` ||
|| **EVENT_MESSAGE_ADD**
[`string`](../data-types.md) | URL for the event handler [ONIMBOTMESSAGEADD](./messages/events/on-imbot-message-add.md) ||
|| **EVENT_MESSAGE_UPDATE**
[`string`](../data-types.md) | URL for the event handler [ONIMBOTMESSAGEUPDATE](./messages/events/on-imbot-message-update.md) ||
|| **EVENT_MESSAGE_DELETE**
[`string`](../data-types.md) | URL for the event handler [ONIMBOTMESSAGEDELETE](./messages/events/on-imbot-message-delete.md) ||
|| **EVENT_WELCOME_MESSAGE**
[`string`](../data-types.md) | URL for the event handler [ONIMBOTJOINCHAT](./chats/events/on-imbot-join-chat.md) ||
|| **EVENT_BOT_DELETE**
[`string`](../data-types.md) | URL for the event handler [ONIMBOTDELETE](./events/on-imbot-delete.md) ||
|| **PROPERTIES**
[`object`](../data-types.md) | Properties of the chatbot profile. The structure of the object is described in detail [below](#fields-properties) ||
|#

{% note warning "" %}

The method `imbot.update` does not support changing the fields `TYPE` and `OPENLINE`.

{% endnote %}

### FIELDS.PROPERTIES Parameter {#fields-properties}

#|
|| **Name**
`Type` | **Description** ||
|| **NAME**
[`string`](../data-types.md) | The name of the chatbot. If both values `NAME` and `LAST_NAME` are provided, they must not be empty at the same time ||
|| **LAST_NAME**
[`string`](../data-types.md) | The last name of the chatbot. If both values `NAME` and `LAST_NAME` are provided, they must not be empty at the same time ||
|| **COLOR**
[`string`](../data-types.md) | The color of the chatbot for the mobile interface: `RED`, `GREEN`, `MINT`, `LIGHT_BLUE`, `DARK_BLUE`, `PURPLE`, `AQUA`, `PINK`, `LIME`, `BROWN`, `AZURE`, `KHAKI`, `SAND`, `MARENGO`, `GRAY`, `GRAPHITE` ||
|| **EMAIL**
[`string`](../data-types.md) | Email for contacting the chatbot. The bot is created as a user, so the bot's email must not match the email of a real Bitrix24 user. This will help avoid account conflicts ||
|| **PERSONAL_BIRTHDAY**
[`string`](../data-types.md) | Birthday in the format `YYYY-MM-DD` ||
|| **WORK_POSITION**
[`string`](../data-types.md) | Position or description of the chatbot ||
|| **PERSONAL_WWW**
[`string`](../data-types.md) | Link to the website ||
|| **PERSONAL_GENDER**
[`string`](../data-types.md) | Gender, acceptable values: `M` or `F` ||
|| **PERSONAL_PHOTO**
[`file`](../data-types.md) | Avatar of the chatbot in [Base64](../files/how-to-upload-files.md) format

The image size must not exceed the limit of 5000x5000 ||
|#

{% note info "" %}

To update the bot, provide at least one parameter: a field in `FIELDS` or an event handler URL. If all parameters are empty, the method will return an error `WRONG_REQUEST`.

{% endnote %}

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"BOT_ID":39,"FIELDS":{"CODE":"newbot_v2","EVENT_HANDLER":"https://example.com/bot/events","PROPERTIES":{"NAME":"UpdatedBot","WORK_POSITION":"Updated description"}}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"BOT_ID":39,"FIELDS":{"CODE":"newbot_v2","EVENT_HANDLER":"https://example.com/bot/events","PROPERTIES":{"NAME":"UpdatedBot"}},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.update
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.update', {
        BOT_ID: 39,
        FIELDS: {
          CODE: 'newbot_v2',
          EVENT_HANDLER: 'https://example.com/bot/events',
          PROPERTIES: {
            NAME: 'UpdatedBot',
            WORK_POSITION: 'Updated description',
          },
        },
      });

      const { result } = response.getData();
      console.log('Updated:', result);
    } catch (error) {
      console.error('Error updating bot:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.update',
                [
                    'BOT_ID' => 39,
                    'FIELDS' => [
                        'CODE' => 'newbot_v2',
                        'EVENT_HANDLER' => 'https://example.com/bot/events',
                        'PROPERTIES' => [
                            'NAME' => 'UpdatedBot',
                            'WORK_POSITION' => 'Updated description',
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Updated: ' . ($result->data() ? 'true' : 'false');
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error updating bot: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.update',
        {
            BOT_ID: 39,
            FIELDS: {
                CODE: 'newbot_v2',
                EVENT_HANDLER: 'https://example.com/bot/events',
                PROPERTIES: {
                    NAME: 'UpdatedBot',
                    WORK_POSITION: 'Updated description',
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
        'imbot.update',
        [
            'BOT_ID' => 39,
            'FIELDS' => [
                'CODE' => 'newbot_v2',
                'EVENT_HANDLER' => 'https://example.com/bot/events',
                'PROPERTIES' => [
                    'NAME' => 'UpdatedBot',
                    'WORK_POSITION' => 'Updated description',
                ],
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Updated: ' . ($result['result'] ? 'true' : 'false');
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
[`boolean`](../data-types.md) | `true` if the bot was updated without error ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "WRONG_REQUEST",
    "error_description": "Update fields can't be empty"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization. | The method was called with session authorization instead of OAuth or webhook ||
|| `ACCESS_DENIED` | Access denied! Client ID not specified | Unable to determine the application: `clientId` authorization is missing and `CLIENT_ID` was not provided ||
|| `BOT_ID_ERROR` | Bot not found | Bot not found ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | The provided `BOT_ID` belongs to another application ||
|| `EVENT_MESSAGE_ADD_ERROR` | Wrong handler URL | An invalid handler URL was provided for `EVENT_MESSAGE_ADD` ||
|| `EVENT_MESSAGE_UPDATE_ERROR` | Wrong handler URL | An invalid handler URL was provided for `EVENT_MESSAGE_UPDATE` ||
|| `EVENT_MESSAGE_DELETE_ERROR` | Wrong handler URL | An invalid handler URL was provided for `EVENT_MESSAGE_DELETE` ||
|| `EVENT_WELCOME_MESSAGE_ERROR` | Wrong handler URL | An invalid handler URL was provided for `EVENT_WELCOME_MESSAGE` ||
|| `EVENT_BOT_DELETE_ERROR` | Wrong handler URL | An invalid handler URL was provided for `EVENT_BOT_DELETE` ||
|| `NAME_ERROR` | Bot name isn't specified | Both fields `NAME` and `LAST_NAME` in `PROPERTIES` are empty ||
|| `WRONG_REQUEST` | Update fields can't be empty | No fields or handlers were provided for the update ||
|| `WRONG_REQUEST` | Bot can't be updated | The bot cannot be updated ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imbot-register.md)
- [{#T}](./imbot-unregister.md)
- [{#T}](./imbot-bot-list.md)
- [{#T}](./events/index.md)
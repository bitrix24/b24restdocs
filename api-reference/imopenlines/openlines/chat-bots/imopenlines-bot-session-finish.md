# Finish Dialog imopenlines.bot.session.finish

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md), [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: application user with a registered chat-bot

The method `imopenlines.bot.session.finish` ends the current dialog of the open line on behalf of the application's chat-bot.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the chat whose dialog needs to be finished.

The chat identifier can be obtained using the methods [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) or [imopenlines.crm.chat.get](../chats/imopenlines-crm-chat-get.md) ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks.

Pass:
- the same `CLIENT_ID` that was specified when registering the chat-bot using the method [imbot.register](../../../chat-bots/outdated/bots/imbot-register.md)
- or the value of the `botToken` parameter provided during the registration of the chat-bot using the method [imbot.v2.Bot.register](../../../chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112,"CLIENT_ID":"**put_your_client_id_or_bot_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.bot.session.finish
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imopenlines.bot.session.finish
  ```

- JS

  ```js
  try {
    const response = await $b24.callMethod('imopenlines.bot.session.finish', {
      CHAT_ID: 112,
    });

    const { result } = response.getData();
    console.log('Session finished:', result);
  } catch (error) {
    console.error('Error finishing session:', error);
  }
  ```

- PHP

  ```php
  try {
      $response = $b24Service
          ->core
          ->call(
              'imopenlines.bot.session.finish',
              [
                  'CHAT_ID' => 112,
              ]
          );

      $result = $response
          ->getResponseData()
          ->getResult();

      echo 'Session finished: ' . var_export($result, true);
  } catch (Throwable $exception) {
      error_log($exception->getMessage());
      echo 'Error finishing session: ' . $exception->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'imopenlines.bot.session.finish',
      {
          CHAT_ID: 112,
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
      'imopenlines.bot.session.finish',
      [
          'CHAT_ID' => 112,
      ]
  );

  if (!empty($result['error'])) {
      echo 'Error: ' . $result['error_description'];
  } else {
      echo 'Session finished: ' . var_export($result['result'], true);
  }
  ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1728626400.456,
        "finish": 1728626400.539,
        "duration": 0.083,
        "processing": 0.031,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Contains `true` if the dialog was successfully finished ||
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
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided or value is `<= 0` ||
|| `400` | `BOT_ID_ERROR` | Bot not found | No registered chat-bot found in the application ||
|| `403` | `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization | Method called with session authorization, which is prohibited for this method ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-bot-session-message-send.md)
- [{#T}](./imopenlines-bot-session-operator.md)
- [{#T}](./imopenlines-bot-session-transfer.md)
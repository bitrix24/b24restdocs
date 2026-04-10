# Switch Dialog to Free Operator imopenlines.bot.session.operator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md), [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imopenlines.bot.session.operator` switches the dialog from the chatbot to a free operator in the open line.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | The identifier of the chat to be passed to the free operator.

The chat identifier can be obtained using the methods [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) or [imopenlines.crm.chat.get](../chats/imopenlines-crm-chat-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.bot.session.operator
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imopenlines.bot.session.operator
  ```

- JS

  ```js
  try {
    const response = await $b24.callMethod('imopenlines.bot.session.operator', {
      CHAT_ID: 112,
    });

    const { result } = response.getData();
    console.log('Session redirected to free operator:', result);
  } catch (error) {
    console.error('Error redirecting session:', error);
  }
  ```

- PHP

  ```php
  try {
      $response = $b24Service
          ->core
          ->call(
              'imopenlines.bot.session.operator',
              [
                  'CHAT_ID' => 112,
              ]
          );

      $result = $response
          ->getResponseData()
          ->getResult();

      echo 'Session redirected: ' . var_export($result, true);
  } catch (Throwable $exception) {
      error_log($exception->getMessage());
      echo 'Error redirecting session: ' . $exception->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'imopenlines.bot.session.operator',
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
      'imopenlines.bot.session.operator',
      [
          'CHAT_ID' => 112,
      ]
  );

  if (!empty($result['error'])) {
      echo 'Error: ' . $result['error_description'];
  } else {
      echo 'Session redirected: ' . var_export($result['result'], true);
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
[`boolean`](../../../data-types.md) | Contains `true` if the dialog was successfully passed to the free operator ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "WRONG_CHAT",
    "error_description": "Operator is not a bot"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided or value `<= 0` ||
|| `400` | `WRONG_CHAT` | Operator is not a bot | The specified dialog is already being handled by an operator, not a chatbot ||
|| `403` | `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization | The method was called with session authorization for which it is prohibited ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-bot-session-message-send.md)
- [{#T}](./imopenlines-bot-session-transfer.md)
- [{#T}](./imopenlines-bot-session-finish.md)
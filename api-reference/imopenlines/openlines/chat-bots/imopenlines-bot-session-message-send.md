# Send an Automatic Message in the imopenlines.bot.session.message.send Dialog

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md), [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imopenlines.bot.session.message.send` sends an automatic message from the application chatbot to the open line dialog.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | The identifier of the chat to which the method sends the message.

The chat identifier can be obtained using the methods [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) or [imopenlines.crm.chat.get](../chats/imopenlines-crm-chat-get.md) ||
|| **NAME**
[`string`](../../../data-types.md) | The code for the auto-response type. 

Allowed values:
- `WELCOME` — sends an automatic greeting from the Open Line settings; the `MESSAGE` parameter is ignored in this mode
- `DEFAULT` — sends the text from the `MESSAGE` parameter ||
|| **MESSAGE**
[`string`](../../../data-types.md) | The message text for the `NAME=DEFAULT` mode. By default, an empty string is used.

If `NAME=DEFAULT` is passed with an empty `MESSAGE`, no new message will be added to the chat ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112,"NAME":"DEFAULT","MESSAGE":"Hello! How can I assist you?"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.bot.session.message.send
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112,"NAME":"DEFAULT","MESSAGE":"Hello! How can I assist you?","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imopenlines.bot.session.message.send
  ```

- JS

  ```js
  try {
    const response = await $b24.callMethod('imopenlines.bot.session.message.send', {
      CHAT_ID: 112,
      NAME: 'DEFAULT',
      MESSAGE: 'Hello! How can I assist you?',
    });

    const { result } = response.getData();
    console.log('Auto message sent:', result);
  } catch (error) {
    console.error('Error sending auto message:', error);
  }
  ```

- PHP

  ```php
  try {
      $response = $b24Service
          ->core
          ->call(
              'imopenlines.bot.session.message.send',
              [
                  'CHAT_ID' => 112,
                  'NAME' => 'DEFAULT',
                  'MESSAGE' => 'Hello! How can I assist you?',
              ]
          );

      $result = $response
          ->getResponseData()
          ->getResult();

      echo 'Auto message sent: ' . var_export($result, true);
  } catch (Throwable $exception) {
      error_log($exception->getMessage());
      echo 'Error sending auto message: ' . $exception->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'imopenlines.bot.session.message.send',
      {
          CHAT_ID: 112,
          NAME: 'DEFAULT',
          MESSAGE: 'Hello! How can I assist you?',
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
      'imopenlines.bot.session.message.send',
      [
          'CHAT_ID' => 112,
          'NAME' => 'DEFAULT',
          'MESSAGE' => 'Hello! How can I assist you?',
      ]
  );

  if (!empty($result['error'])) {
      echo 'Error: ' . $result['error_description'];
  } else {
      echo 'Auto message sent: ' . var_export($result['result'], true);
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
[`boolean`](../../../data-types.md) | Flag indicating the successful call of the method. The method does not return an indication of the actual creation of the message in the chat ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided or value `<= 0` provided ||
|| `403` | `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization | Method called with session authorization for which it is prohibited ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-bot-session-operator.md)
- [{#T}](./imopenlines-bot-session-transfer.md)
- [{#T}](./imopenlines-bot-session-finish.md)
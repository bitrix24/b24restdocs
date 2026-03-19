# Transfer a Dialogue to an Operator or Queue using imopenlines.bot.session.transfer

> Scope: [`imopenlines`](../../../scopes/permissions.md), [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: application user with a registered chat bot

The method `imopenlines.bot.session.transfer` transfers a dialogue from the chat bot in an open line to a specified operator or queue.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | The identifier of the chat whose dialogue needs to be transferred.

The chat identifier can be obtained using the methods [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) or [imopenlines.crm.chat.get](../chats/imopenlines-crm-chat-get.md) ||
|| **USER_ID**
[`integer`](../../../data-types.md) | The identifier of the employee to whom the dialogue should be transferred.

The user identifier can be obtained using the methods [user.get](../../../user/user-get.md) or [user.search](../../../user/user-search.md) ||
|| **QUEUE_ID**
[`integer`](../../../data-types.md) | The identifier of the queue to which the dialogue should be transferred.

Pass the value of the `ID` field from the response of the method [imopenlines.config.list.get](../imopenlines-config-list-get.md).

The `QUEUE` field from the response of [imopenlines.config.list.get](../imopenlines-config-list-get.md) contains `USER_ID` of the queue employees and is not suitable as `QUEUE_ID` ||
|| **TRANSFER_ID**
[`string`](../../../data-types.md)\|[`integer`](../../../data-types.md) | Universal transfer destination parameter.

Allowed formats: `USER_ID` of the employee or the string `queue<QUEUE_ID>`.

The user identifier can be obtained using the methods [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md), and the queue identifier can be obtained using the method [imopenlines.config.list.get](../imopenlines-config-list-get.md) ||
|| **LEAVE**
[`boolean`](../../../data-types.md) | Flag indicating the presence of the chat bot in the dialogue after the transfer. 

Allowed values:
- `Y` — the bot leaves the chat immediately
- `N` — the bot remains until the transfer is confirmed

By default, `N` is used ||
|#

It is recommended to pass only one destination parameter: `USER_ID`, `QUEUE_ID`, or `TRANSFER_ID`. If multiple parameters are passed simultaneously, the method uses the following priority: `USER_ID` -> `QUEUE_ID` -> `TRANSFER_ID`. When passing `TRANSFER_ID`, the method converts it to `USER_ID` or `QUEUE_ID`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112,"USER_ID":12,"LEAVE":"N"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.bot.session.transfer
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112,"USER_ID":12,"LEAVE":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imopenlines.bot.session.transfer
  ```

- JS

  ```js
  try {
    const response = await $b24.callMethod('imopenlines.bot.session.transfer', {
      CHAT_ID: 112,
      USER_ID: 12,
      LEAVE: 'N',
    });

    const { result } = response.getData();
    console.log('Session transferred:', result);
  } catch (error) {
    console.error('Error transferring session:', error);
  }
  ```

- PHP

  ```php
  try {
      $response = $b24Service
          ->core
          ->call(
              'imopenlines.bot.session.transfer',
              [
                  'CHAT_ID' => 112,
                  'USER_ID' => 12,
                  'LEAVE' => 'N',
              ]
          );

      $result = $response
          ->getResponseData()
          ->getResult();

      echo 'Session transferred: ' . var_export($result, true);
  } catch (Throwable $exception) {
      error_log($exception->getMessage());
      echo 'Error transferring session: ' . $exception->getMessage();
  }
  ```

- BX24.js

  ```js
  BX24.callMethod(
      'imopenlines.bot.session.transfer',
      {
          CHAT_ID: 112,
          USER_ID: 12,
          LEAVE: 'N',
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
      'imopenlines.bot.session.transfer',
      [
          'CHAT_ID' => 112,
          'USER_ID' => 12,
          'LEAVE' => 'N',
      ]
  );

  if (!empty($result['error'])) {
      echo 'Error: ' . $result['error_description'];
  } else {
      echo 'Session transferred: ' . var_export($result['result'], true);
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
[`boolean`](../../../data-types.md) | Contains `true` if the dialogue was successfully transferred ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "TRANSFER_ID_EMPTY",
    "error_description": "Queue ID or User ID can't be empty"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided or value `<= 0` ||
|| `400` | `USER_ID_EMPTY` | User ID can't be empty | `USER_ID` provided with an empty value or value `<= 0` ||
|| `400` | `QUEUE_ID_EMPTY` | QUEUE ID can't be empty | `QUEUE_ID` provided with an empty value or value `<= 0` ||
|| `400` | `TRANSFER_ID_EMPTY` | Queue ID or User ID can't be empty | Neither `USER_ID` nor `QUEUE_ID` provided, and `TRANSFER_ID` not set ||
|| `400` | `BOT_ID_ERROR` | Bot not found | No registered chat bot found in the application ||
|| `400` | `OPERATOR_WRONG` | You cannot redirect to this operator | Transfer to the specified operator or queue is not available ||
|| `403` | `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization | Method called with session authorization for which it is prohibited ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-bot-session-message-send.md)
- [{#T}](./imopenlines-bot-session-operator.md)
- [{#T}](./imopenlines-bot-session-finish.md)
# Transfer a Dialogue to an Operator or Queue using imopenlines.bot.session.transfer

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md), [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: application user with a registered chat bot

The method `imopenlines.bot.session.transfer` transfers a dialogue from the chat bot in an open line to a specified operator or queue.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

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

The `QUEUE` field from the response of [imopenlines.config.list.get](../imopenlines-config-list-get.md) contains `USER_ID` of the employees in the queue and is not suitable as `QUEUE_ID` ||
|| **TRANSFER_ID** 
[`string`](../../../data-types.md)\|[`integer`](../../../data-types.md) | Universal transfer destination parameter.

Allowed formats: `USER_ID` of the employee or the string `queue<QUEUE_ID>`.

The user identifier can be obtained using the methods [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md), and the queue identifier can be obtained using the method [imopenlines.config.list.get](../imopenlines-config-list-get.md) ||
|| **LEAVE** 
[`boolean`](../../../data-types.md) | Flag indicating the presence of the chat bot in the dialogue after the transfer.

Allowed values:
- `Y` — the bot leaves the chat immediately
- `N` — the bot stays until the transfer is confirmed

By default, `N` is used ||
|| **CLIENT_ID** 
[`string`](../../../data-types.md) | This parameter is mandatory only for webhooks.

Pass:
- the same `CLIENT_ID` that was specified when registering the chat bot using the method [imbot.register](../../../chat-bots/outdated/bots/imbot-register.md)
- or the value of the `botToken` parameter passed during the registration of the chat bot using the method [imbot.v2.Bot.register](../../../chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md) ||
|#

It is recommended to pass only one destination parameter: `USER_ID`, `QUEUE_ID`, or `TRANSFER_ID`. If multiple parameters are passed simultaneously, the method uses the following priority: `USER_ID` -> `QUEUE_ID` -> `TRANSFER_ID`. When passing `TRANSFER_ID`, the method converts it to `USER_ID` or `QUEUE_ID`.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":112,"USER_ID":12,"LEAVE":"N","CLIENT_ID":"**put_your_client_id_or_bot_token_here**"}' \
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

- JS (TS)

  ```ts
  // This snippet is an ES module: top-level await requires type="module" or a bundler.
  // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
  import { Text } from '@bitrix24/b24jssdk'
  import type { B24Frame } from '@bitrix24/b24jssdk'

  declare const $b24: B24Frame

  try {
    const response = await $b24.actions.v2.call.make<boolean>({
      method: 'imopenlines.bot.session.transfer',
      params: {
        CHAT_ID: 112,
        USER_ID: 12,
        LEAVE: 'N',
      },
      requestId: Text.getUuidRfc4122()
    })

    // The payload is available only on a successful response
    if (!response.isSuccess) {
      console.error(response.getErrorMessages().join('; '))
    } else {
      const result = response.getData()!.result
      console.info('Session transferred:', result)
    }
  } catch (error) {
    // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
    console.error(error)
  }
  ```

- JS (UMD)

  ```html
  <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
  <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
  <script>
    async function transferSession() {
      try {
        // Initialize the SDK inside a Bitrix24 frame
        const $b24 = await B24Js.initializeB24Frame()

        const response = await $b24.actions.v2.call.make({
          method: 'imopenlines.bot.session.transfer',
          params: {
            CHAT_ID: 112,
            USER_ID: 12,
            LEAVE: 'N',
          },
          requestId: B24Js.Text.getUuidRfc4122()
        })

        // The payload is available only on a successful response
        if (!response.isSuccess) {
          console.error(response.getErrorMessages().join('; '))
          return
        }

        const result = response.getData().result
        console.info('Session transferred:', result)
      } catch (error) {
        // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
        console.error(error)
      }
    }

    document.addEventListener('DOMContentLoaded', transferSession)
  </script>
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

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided or value is `<= 0` ||
|| `400` | `USER_ID_EMPTY` | User ID can't be empty | `USER_ID` provided with an empty value or value `<= 0` ||
|| `400` | `QUEUE_ID_EMPTY` | QUEUE ID can't be empty | `QUEUE_ID` provided with an empty value or value `<= 0` ||
|| `400` | `TRANSFER_ID_EMPTY` | Queue ID or User ID can't be empty | Neither `USER_ID` nor `QUEUE_ID` provided, and `TRANSFER_ID` not specified ||
|| `400` | `BOT_ID_ERROR` | Bot not found | No registered chat bot found in the application ||
|| `400` | `OPERATOR_WRONG` | You cannot redirect to this operator | Transfer to the specified operator or queue is not available ||
|| `403` | `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization | Method called with session authorization, which is prohibited for this method ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-bot-session-message-send.md)
- [{#T}](./imopenlines-bot-session-operator.md)
- [{#T}](./imopenlines-bot-session-finish.md)
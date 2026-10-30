# Unregister a Bot imbot.v2.Bot.unregister

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Bot.unregister` deletes the bot. It replaces the [imbot.unregister](../../../outdated/bots/imbot-unregister.md) method of the first API version; the parameter mapping is in the [Migration Table](../../migration.md#methods).

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#| 
|| **Name** 
`Type` | **Description** ||
|| **botId*** 
[`integer`](../../../../data-types.md) | Bot ID. It is returned by the [imbot.v2.Bot.register](./bot-register.md) method in the `result.bot.id` field ||
|| **botToken** 
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat-bot registration ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Bot.unregister
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Bot.unregister
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BotUnregisterResult = {
      result: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<BotUnregisterResult>({
        method: 'imbot.v2.Bot.unregister',
        params: {
          botId: 456,
          botToken: 'my_bot_token',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Bot deleted:', result.result)
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
      async function unregisterBot() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imbot.v2.Bot.unregister',
            params: {
              botId: 456,
              botToken: 'my_bot_token',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Bot deleted:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', unregisterBot)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imbot.v2.bot.unregister(
            bot_id=456,
            bot_token="my_bot_token",
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.v2.Bot.unregister',
                [
                    'botId' => 456,
                    'botToken' => 'my_bot_token',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: ' . print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Bot.unregister',
        {
            botId: 456,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log('Bot deleted:', result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.v2.Bot.unregister',
        [
            'botId' => 456,
            'botToken' => 'my_bot_token',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Bot deleted';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imbot.v2.Bot.unregister", b24.Params{
    	"botId":    456,
    	"botToken": "my_bot_token",
    })
    if err != nil {
    	return fmt.Errorf("imbot.v2.Bot.unregister: %w", err)
    }

    var item struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true
    },
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T08:00:00+02:00",
        "date_finish": "2024-10-11T08:00:00+02:00"
    }
}
```

## Returned Data

#| 
|| **Name** 
`Type` | **Description** ||
|| **result** 
[`object`](../../../../data-types.md) | Result of the operation ||
|| **result.result** 
[`boolean`](../../../../data-types.md) | `true` if deletion was successful ||
|| **time** 
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Automatic Removal of Event Subscriptions {#event-subscriptions}

After successfully executing `Bot.unregister`, all subscriptions of the deleted bot to events `ONIMBOTV2*` **are automatically removed** — regardless of whether the bot was in `webhook` or `fetch` mode. An additional call to [event.unbind](../../../../events/event-unbind.md) is not required.

{% note info "" %}

**OAuth applications with multiple bots:** if there are other active bots under the same `clientId` in the application, the automatic cleanup is skipped to avoid affecting their subscriptions. Subscriptions will be removed when the last bot of the application is deleted or along with the application itself when processing the `OnRestAppDelete` event.

{% endnote %}

## Error Handling

HTTP status: **400**

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
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token not specified (botToken is required for webhook auth) | `botToken` is not provided. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | botId is required | `botId` is not provided ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot was installed by another rest application | The bot is registered by another application or with a different `botToken` ||
|| `BOT_UNREGISTER_FAILED` | Bot unregister failed | Error deleting the bot ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](./bot-register.md)
- [{#T}](./bot-get.md)
- [{#T}](./bot-list.md)
- [{#T}](./bot-update.md)
- [{#T}](../../migration.md)
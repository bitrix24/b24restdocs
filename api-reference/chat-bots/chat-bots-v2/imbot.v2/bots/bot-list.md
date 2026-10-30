# List of Application Bots imbot.v2.Bot.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: authorized user

The `imbot.v2.Bot.list` method returns a list of bots for the current application in an extended format. When called via a webhook, it returns the bots registered with the passed `botToken`. It replaces the [imbot.bot.list](../../../outdated/bots/imbot-bot-list.md) method of the first API version; the parameter mapping is in the [Migration Table](../../migration.md#methods).

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chatbot registration ||
|| **filter**
[`object`](../../../../data-types.md) | Filter for results.

Available filter fields:
- `type` — type of the bot, a string, for example `bot` or `supervisor`. For a description of the types, see [Bot Types](../../index.md#bot-types) ||
|| **limit**
[`integer`](../../../../data-types.md) | Number of bots per page. Default is `50`. With a value of `0` or less, the method returns all bots of the application ||
|| **offset**
[`integer`](../../../../data-types.md) | Offset for pagination. Default is `0`. To retrieve the next page, increase `offset` by `limit` while `hasNextPage` in the response is `true`. A negative value causes an error ||
|#

## Code Examples

{% include [Note on Examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botToken":"my_bot_token","filter":{"type":"bot"},"limit":10}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Bot.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"filter":{"type":"bot"},"limit":10,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Bot.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Minimal shape of result; the API returns the full Bot and User objects
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BotListResult = {
      bots: Array<{
        id: number
        code: string
        type: string
      }>
      users: Array<{
        id: number
        name: string
      }>
      hasNextPage: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<BotListResult>({
        method: 'imbot.v2.Bot.list',
        params: {
          botToken: 'my_bot_token',
          filter: { type: 'bot' },
          limit: 10,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Bots:', result.bots.length, '| Has next page:', result.hasNextPage)
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
      async function listBots() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imbot.v2.Bot.list',
            params: {
              botToken: 'my_bot_token',
              filter: { type: 'bot' },
              limit: 10,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Bots:', result.bots.length, '| Has next page:', result.hasNextPage)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listBots)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imbot.v2.bot.list(
            bot_token="my_bot_token",
            filter={
                "type": "bot",
            },
            limit=10,
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
                'imbot.v2.Bot.list',
                [
                    'botToken' => 'my_bot_token',
                    'filter' => ['type' => 'bot'],
                    'limit' => 10,
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
        'imbot.v2.Bot.list',
        {
            filter: { type: 'bot' },
            limit: 10,
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
        'imbot.v2.Bot.list',
        [
            'botToken' => 'my_bot_token',
            'filter' => ['type' => 'bot'],
            'limit' => 10,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        foreach ($result['result']['bots'] as $bot) {
            echo $bot['id']. ': '. $bot['code']. "\n";
        }
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imbot.v2.Bot.list", b24.Params{
    	"botToken": "my_bot_token",
    	"filter": b24.Params{
    		"type": "bot",
    	},
    	"limit": 10,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imbot.v2.Bot.list: %w", err)
    }

    var item struct {
    	HasNextPage bool `json:"hasNextPage"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.HasNextPage)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "bots": [
            {
                "id": 456,
                "code": "support_bot",
                "type": "bot",
                "isHidden": false,
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
            }
        ],
        "users": [
            {
                "id": 456,
                "active": true,
                "name": "Support Bot",
                "bot": true,
                "type": "bot"
            }
        ],
        "hasNextPage": false
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
[`object`](../../../../data-types.md) | Result of the request ||
|| **result.bots**
[`Bot[]`](../../entities.md#bot) | Array of bots in extended format [(detailed description)](#bot-object) ||
|| **result.users**
[`User[]`](../../entities.md#user) | Array of related users [(detailed description)](#user-object) ||
|| **result.hasNextPage**
[`boolean`](../../../../data-types.md) | Is there a next page ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the Bot Object {#bot-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the bot ||
|| **code**
[`string`](../../../../data-types.md) | Symbolic code of the bot ||
|| **type**
[`string`](../../../../data-types.md) | Type of the bot ||
|| **isHidden**
[`boolean`](../../../../data-types.md) | Bot is hidden from the contact list ||
|| **isSupportOpenline**
[`boolean`](../../../../data-types.md) | Bot supports open channels ||
|| **isReactionsEnabled**
[`boolean`](../../../../data-types.md) | Reactions are enabled for bot messages ||
|| **backgroundId**
[```string|null```](../../../../data-types.md) | Chat background ID or `null` ||
|| **language**
[`string`](../../../../data-types.md) | Language of the bot ||
|| **moduleId**
[`string`](../../../../data-types.md) | Module identifier ||
|| **eventMode**
[`string`](../../../../data-types.md) | Event delivery mode: `webhook` or `fetch` ||
|| **countMessage**
[`integer`](../../../../data-types.md) | Number of messages sent by the bot ||
|| **countCommand**
[`integer`](../../../../data-types.md) | Number of registered commands ||
|| **countChat**
[`integer`](../../../../data-types.md) | Number of chats for the bot ||
|| **countUser**
[`integer`](../../../../data-types.md) | Number of users who interacted with the bot ||
|#

### Fields of the User Object {#user-object}

The table and the response example show the main fields. The method returns the full User object.

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the user ||
|| **active**
[`boolean`](../../../../data-types.md) | User is active ||
|| **name**
[`string`](../../../../data-types.md) | User's first and last name ||
|| **bot**
[`boolean`](../../../../data-types.md) | Indicates if the user is a bot ||
|| **type**
[`string`](../../../../data-types.md) | Type of user ||
|#

Complete description of all object fields is available on the [Objects and Fields](../../entities.md) page.

{% note info "" %}

If the application has no bots, or an unknown `filter.type` value or a different `botToken` is passed, the method returns an empty `bots` array without an error.

{% endnote %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "BOT_TOKEN_NOT_SPECIFIED",
    "error_description": "Bot token not specified (botToken is required for webhook auth)"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token not specified (botToken is required for webhook auth) | `botToken` is not specified. Required for webhook authorization ||
|| `100` | Invalid value {…} to match with parameter {…}. Should be value of type … | `limit` or `offset` is not an integer, or `filter` is not an object ||
|| `400` | Internal server error. | A negative `offset` value is passed ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](./bot-register.md)
- [{#T}](./bot-get.md)
- [{#T}](./bot-update.md)
- [{#T}](./bot-unregister.md)
- [{#T}](../../migration.md)
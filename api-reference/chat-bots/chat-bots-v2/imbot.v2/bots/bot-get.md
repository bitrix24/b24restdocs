# Get Information About the Bot imbot.v2.Bot.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: authorized user

The method `imbot.v2.Bot.get` returns information about the bot. It is used to verify the bot's installation.

For the owner application, the method returns the extended format of the Bot object. For other applications and for requests with a different `botToken`, it returns the brief format, without the [Additional Owner-Only Fields](#bot-owner-fields).

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#| 
|| **Name** 
`Type` | **Description** ||
|| **botId** 
[`integer`](../../../../data-types.md) | Bot ID from the `result.bot.id` field of the [imbot.v2.Bot.register](./bot-register.md) response. Required if `code` is not specified ||
|| **code** 
[`string`](../../../../data-types.md) | Bot code. Required if `botId` is not specified ||
|| **botToken** 
[`string`](../../../../data-types.md) | Unique bot authorization token. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chatbot registration. With a different token, the method returns the bot in the brief format ||
|#

{% note info "" %}

You must provide one of the parameters: `botId` or `code`.

{% endnote %}

## Code Examples

{% include [Note on Examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botToken":"my_bot_token","code":"support_bot"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Bot.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"code":"support_bot","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Bot.get
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
    type BotGetResult = {
      bot: {
        id: number
        code: string
        type: string
      }
      users: Array<{
        id: number
        name: string
      }>
    }

    try {
      const response = await $b24.actions.v2.call.make<BotGetResult>({
        method: 'imbot.v2.Bot.get',
        params: {
          botToken: 'my_bot_token',
          code: 'support_bot',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Bot ID:', result.bot.id, '| Code:', result.bot.code)
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
      async function getBot() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imbot.v2.Bot.get',
            params: {
              botToken: 'my_bot_token',
              code: 'support_bot',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Bot ID:', result.bot.id, '| Code:', result.bot.code)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getBot)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imbot.v2.bot.get(
            bot_token="my_bot_token",
            code="support_bot",
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
                'imbot.v2.Bot.get',
                [
                    'botToken' => 'my_bot_token',
                    'code' => 'support_bot',
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
        'imbot.v2.Bot.get',
        {
            code: 'support_bot',
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
        'imbot.v2.Bot.get',
        [
            'botToken' => 'my_bot_token',
            'code' => 'support_bot',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'Bot ID: '. $result['result']['bot']['id'];
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imbot.v2.Bot.get", b24.Params{
    	"botToken": "my_bot_token",
    	"code":     "support_bot",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imbot.v2.Bot.get: %w", err)
    }

    // The response shape is shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "bot": {
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
        },
        "users": [
            {
                "id": 456,
                "active": true,
                "name": "Support Bot",
                "bot": true,
                "type": "bot"
            }
        ]
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
|| **result.bot** 
[`Bot`](../../entities.md#bot) | Bot object. Extended format for the owner, brief format for others [(detailed description)](#bot-object) ||
|| **result.users** 
[`User[]`](../../entities.md#user) | Array of related users [(detailed description)](#user-object) ||
|| **time** 
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the Bot Object {#bot-object}

These fields are returned in both formats.

#| 
|| **Field** 
`Type` | **Description** ||
|| **id** 
[`integer`](../../../../data-types.md) | Bot identifier ||
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
|#

#### Additional Fields (Owner Only) {#bot-owner-fields}

These fields are returned only in the extended format: to the application that registered the bot, or in response to a webhook request with the same `botToken`.

#|
|| **Field**
`Type` | **Description** ||
|| **moduleId** 
[`string`](../../../../data-types.md) | Module identifier ||
|| **eventMode** 
[`string`](../../../../data-types.md) | Event delivery mode: `webhook` or `fetch` ||
|| **countMessage** 
[`integer`](../../../../data-types.md) | Number of messages sent by the bot ||
|| **countCommand** 
[`integer`](../../../../data-types.md) | Number of registered commands ||
|| **countChat** 
[`integer`](../../../../data-types.md) | Number of bot chats ||
|| **countUser** 
[`integer`](../../../../data-types.md) | Number of users interacting with the bot ||
|#

### Fields of the User Object {#user-object}

The table and the response example show the main fields. The method returns the full User object.

#| 
|| **Field** 
`Type` | **Description** ||
|| **id** 
[`integer`](../../../../data-types.md) | User identifier ||
|| **active** 
[`boolean`](../../../../data-types.md) | User is active ||
|| **name** 
[`string`](../../../../data-types.md) | User's first and last name ||
|| **bot** 
[`boolean`](../../../../data-types.md) | Indicates if the user is a bot ||
|| **type** 
[`string`](../../../../data-types.md) | Type of user ||
|#

Full description of all object fields can be found on the [Objects and Fields](../../entities.md) page.

## Error Handling

HTTP Status: **400**

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
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token not specified (botToken is required for webhook auth) | `botToken` is not specified. Required for webhook authorization ||
|| `PARAMS_REQUIRED` | botId or code is required | Neither `botId` nor `code` is provided ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./bot-register.md)
- [{#T}](./bot-update.md)
- [{#T}](./bot-list.md)
- [{#T}](./bot-unregister.md)
# Get Events from imbot.v2.Event.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Event.get` returns bot events, such as new messages and command calls. It is used by bots with `eventMode: "fetch"` that collect events themselves with requests.

If the bot has a public URL, events can be received in `webhook` mode — how to choose a mode is described in the [Event Delivery Modes](./index.md#event-modes) overview. In `webhook` mode, bot events are not recorded. The method returns an empty `result.events` array, and with `withUserEvents: true`, only user events.

Recorded events are stored for 24 hours. Bitrix24 deletes older events even if the bot has not received them.

{% note info "" %}

Bot events can be received only by the application that registered the bot, or by a webhook with the same `botToken`. The bot has a single queue: events confirmed by one agent will not reach another. Therefore, for several independent agents, register a separate bot for each.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token of the bot — the one specified during registration, or a new one after the token is changed with the [imbot.v2.Bot.update](../bots/bot-update.md) method. Required if the method is called via an inbound webhook, not needed for OAuth ||
|| **offset**
[`integer`](../../../../data-types.md) | Confirms and removes from the queue all events with IDs less than the specified value. Pass `nextOffset` from the previous response. Not needed on the first call. Default is `0`.

If you pass a value greater than `nextOffset`, the method also deletes events you have not received yet ||
|| **limit**
[`integer`](../../../../data-types.md) | Maximum number of events returned (1–1000). Default is `100`. The method replaces an out-of-range value with the nearest limit ||
|| **withUserEvents**
[`boolean`](../../../../data-types.md) | Include user events (`ONIMV2*`) in the response along with bot events (`ONIMBOTV2*`). Default is `false`.

Works if:

- the application has the [`im`](../../../../scopes/permissions.md) scope in addition to `imbot`
- the user on whose behalf the call is made is subscribed via [im.v2.Event.subscribe](../../im.v2/events/event-subscribe.md)

The method returns events of this user only. A single `offset` confirms both bot and user events. The user's event queue is shared with [im.v2.Event.get](../../im.v2/events/event-get.md): events confirmed in this method will no longer be returned by `im.v2.Event.get` ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","offset":1000,"limit":50}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Event.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"offset":1000,"limit":50,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Event.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EventGetResult = {
      events: Array<{
        eventId: number
        type: string
        date: string
        data: Record<string, unknown>
      }>
      nextOffset: number
      hasMore: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<EventGetResult>({
        method: 'imbot.v2.Event.get',
        params: {
          botId: 456,
          botToken: 'my_bot_token',
          offset: 1000,
          limit: 50,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Events:', result.events.length, '| nextOffset:', result.nextOffset, '| hasMore:', result.hasMore)
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
      async function getEvents() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imbot.v2.Event.get',
            params: {
              botId: 456,
              botToken: 'my_bot_token',
              offset: 1000,
              limit: 50,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Events:', result.events.length, '| nextOffset:', result.nextOffset, '| hasMore:', result.hasMore)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEvents)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imbot.v2.event.get(
            bot_id=456,
            bot_token="my_bot_token",
            offset=1000,
            limit=50,
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
                'imbot.v2.Event.get',
                [
                    'botId' => 456,
                    'botToken' => 'my_bot_token',
                    'offset' => 1000,
                    'limit' => 50,
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
        'imbot.v2.Event.get',
        {
            botId: 456,
            botToken: 'my_bot_token',
            offset: 1000,
            limit: 50,
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
        'imbot.v2.Event.get',
        [
            'botId' => 456,
            'botToken' => 'my_bot_token',
            'offset' => 1000,
            'limit' => 50,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        foreach ($result['result']['events'] as $event) {
            echo $event['type'] . ': ' . $event['eventId'] . "\n";
        }
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imbot.v2.Event.get", b24.Params{
    	"botId":    456,
    	"botToken": "my_bot_token",
    	"offset":   1000,
    	"limit":    50,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imbot.v2.Event.get: %w", err)
    }

    var item struct {
    	NextOffset int  `json:"nextOffset"`
    	HasMore    bool `json:"hasMore"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.NextOffset, item.HasMore)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "events": [
            {
                "eventId": 1001,
                "type": "ONIMBOTV2MESSAGEADD",
                "date": "2024-10-11T09:58:12+02:00",
                "data": {
                    "message": {
                        "id": 193,
                        "chatId": 112,
                        "authorId": 129,
                        "date": "2024-10-11T09:58:12+02:00",
                        "text": "Hello! What is the status of order 1024?",
                        "isSystem": false,
                        "params": []
                    },
                    "chat": {
                        "id": 112,
                        "dialogId": "129",
                        "type": "private",
                        "name": ""
                    },
                    "user": {
                        "id": 129,
                        "name": "Anna Schmidt",
                        "bot": false
                    },
                    "language": "en",
                    "additionalMessages": [],
                    "bot": {
                        "id": 456,
                        "code": "support_bot",
                        "type": "bot",
                        "eventMode": "fetch"
                    }
                }
            }
        ],
        "nextOffset": 1002,
        "hasMore": false
    },
    "time": {
        "start": 1728630000.123,
        "finish": 1728630000.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00"
    }
}
```

The `message`, `chat`, `user`, and `bot` objects in the example are shortened. For the full set of fields, see the [ONIMBOTV2MESSAGEADD](./events.md#onimbotv2messageadd) event description.

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Result of the operation ||
|| **result.events**
[`object[]`](../../../../data-types.md) | Array of events ||
|| **result.events[].eventId**
[`integer`](../../../../data-types.md) | Event ID ||
|| **result.events[].type**
[`string`](../../../../data-types.md) | Type of event. List of types described [below](#event-types) ||
|| **result.events[].date**
[`datetime`](../../../../data-types.md) | Date and time of the event ||
|| **result.events[].data**
[`object`](../../../../data-types.md) | Event data. The set of fields depends on `type`; links to the description of each type are in the table [below](#event-types) ||
|| **result.nextOffset**
[`integer`](../../../../data-types.md) | ID from which the next batch starts. Pass it in `offset` of the next call to confirm the received events. If there are no new events, it equals the passed `offset` ||
|| **result.hasMore**
[`boolean`](../../../../data-types.md) | `true` if the method returned as many events as specified in `limit`: more may remain in the queue. Call the method again with `offset` equal to `nextOffset` ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Event Types {#event-types}

#|
|| **Type** | **Description** ||
|| [ONIMBOTV2MESSAGEADD](./events.md#onimbotv2messageadd) | New message to the bot ||
|| [ONIMBOTV2MESSAGEUPDATE](./events.md#onimbotv2messageupdate) | Message edited ||
|| [ONIMBOTV2MESSAGEDELETE](./events.md#onimbotv2messagedelete) | Message deleted ||
|| [ONIMBOTV2JOINCHAT](./events.md#onimbotv2joinchat) | Bot added to a chat ||
|| [ONIMBOTV2DELETE](./events.md#onimbotv2delete) | Bot deleted. In `fetch` mode, the event is not sent: the queue is deleted together with the bot ||
|| [ONIMBOTV2CONTEXTGET](./events.md#onimbotv2contextget) | The bot received the context when a dialog was opened ||
|| [ONIMBOTV2COMMANDADD](./events.md#onimbotv2commandadd) | Slash command invoked ||
|| [ONIMBOTV2REACTIONCHANGE](./events.md#onimbotv2reactionchange) | Reaction changed ||
|#

The bot receives messages addressed to it: in a one-on-one chat, with an `@bot` mention, or as a reply to its message. Bots of type `personal` and `supervisor` receive all user messages in group chats they belong to, except messages from other bots.

Detailed description of the data format for each event: [{#T}](./events.md).

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
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token not specified (botToken is required for webhook auth) | `botToken` is not specified. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | botId is required | `botId` is not specified or is less than 1 ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot was installed by another rest application | The bot was registered by another application or with a different `botToken` ||
|| `SCOPE_ERROR` | Scope 'im' is required to fetch user events | `withUserEvents: true` was passed, but the application does not have the `im` scope ||
|| `AUTH_ERROR` | Authorization required for user events | Failed to determine the current user ||
|| `USER_NOT_SUBSCRIBED` | User is not subscribed. Call im.v2.Event.subscribe first | `withUserEvents: true` was passed, but the user is not subscribed to events via [im.v2.Event.subscribe](../../im.v2/events/event-subscribe.md) ||
|| `100` | Invalid value {abc} to match with parameter {offset}. Should be value of type int. | A non-numeric value was passed in `offset` or `limit` ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [Events imbot.v2](./index.md)
- [{#T}](./events.md)
- [{#T}](../../quick-start.md)
- [{#T}](../../index.md)
- [{#T}](../bots/bot-register.md)
- [{#T}](../messages/chat-message-send.md)
- [{#T}](../../im.v2/events/event-get.md)
- [{#T}](../../im.v2/events/event-subscribe.md)
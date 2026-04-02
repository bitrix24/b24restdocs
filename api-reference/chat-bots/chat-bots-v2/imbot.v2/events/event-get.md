# Get Bot Events imbot.v2.Event.get

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Event.get` retrieves events for the bot in polling agent mode. It is used for bots with `eventMode: "fetch"`.

The method confirms the receipt of events with IDs less than the provided `offset`. On the next call, only new events starting from the confirmed ones are returned.

{% note info "" %}

Only one application can receive events for a specific bot — the one that registered it. If you need multiple independent agents, register a separate bot for each.

{% endnote %}

## When to Use `imbot.v2.Event.get`

This method is suitable if:

- the bot does not have a public URL for incoming HTTP requests
- events should be fetched by a single background agent (worker) on a schedule
- explicit control of the event queue is needed through `offset` and `nextOffset`
- you need to receive bot events `ONIMBOTV2*` and user events `ONIMV2*` in a single stream using `withUserEvents`

## Alternative: Webhook Mode

Instead of polling, you can use webhook mode:

- in [imbot.v2.Bot.register](../bots/bot-register.md), specify `fields.eventMode = webhook`
- set `fields.webhookUrl` to receive incoming events
- [example of a webhook handler](../../index.md#webhook)

Step-by-step setup for both options: [Quick Start](../../quick-start.md).

## Method Parameters

{% include [Note on Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during chat bot registration ||
|| **offset**
[`integer`](../../../../data-types.md) | Confirms all events with IDs less than the specified value. Not passed on the first call ||
|| **limit**
[`integer`](../../../../data-types.md) | Maximum number of events returned (1–1000). Default is `100` ||
|| **withUserEvents**
[`boolean`](../../../../data-types.md) | Include user events (`ONIMV2*`) in the response along with bot events. Default is `false`. More details — [below](#combined-fetch) ||
|#

### Retrieving Bot and User Events {#combined-fetch}

When `withUserEvents: true`, the method returns both bot events `ONIMBOTV2*` and user events `ONIMV2*` in a single response.

Requirements:
- the application must have the scope [`im`](../../../../scopes/permissions.md) in addition to `imbot`
- the current user must be subscribed via [im.v2.Event.subscribe](../../im.v2/events/event-subscribe.md)
- `userId` is determined from the authorization — specifying another user's ID is not possible

One `offset` confirms both bot events and user events simultaneously.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","offset":1000,"limit":50,"withUserEvents":true}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Event.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"offset":1000,"limit":50,"withUserEvents":true,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Event.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Event.get', {
        botId: 456,
        offset: 1000,
        limit: 50,
        withUserEvents: true,
      });

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
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
                    'offset' => 1000,
                    'limit' => 50,
                    'withUserEvents' => true,
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
            offset: 1000,
            limit: 50,
            withUserEvents: true,
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
            'offset' => 1000,
            'limit' => 50,
            'withUserEvents' => true,
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

{% endlist %}

## Code Examples

{% include [Note on Examples](../../../../../_includes/examples.md) %}

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

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Event.get', {
        botId: 456,
        offset: 1000,
        limit: 50,
      });

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
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

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "events": [
            {
                "eventId": 1001,
                "type": "ONIMBOTV2MESSAGEADD",
                "date": "2025-01-15T10:30:00+02:00",
                "data": {}
            }
        ],
        "nextOffset": 1002,
        "hasMore": false
    },
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Result of the operation ||
|| **result.events**
[`object[]`](../../../../data-types.md) | Array of events ||
|| **result.events[].eventId**
[`integer`](../../../../data-types.md) | Event ID. Pass in the next call as `offset` for confirmation ||
|| **result.events[].type**
[`string`](../../../../data-types.md) | Type of event. List of types is described [below](#event-types) ||
|| **result.events[].date**
[`datetime`](../../../../data-types.md) | Date and time of the event ||
|| **result.events[].data**
[`object`](../../../../data-types.md) | Event data. The format depends on the type of event: [event descriptions](./events.md) ||
|| **result.nextOffset**
[`integer`](../../../../data-types.md) | Value for the next call `offset`. Confirms already processed events and shifts the queue cursor ||
|| **result.hasMore**
[`boolean`](../../../../data-types.md) | `true` if there are more unprocessed events ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Event Types {#event-types}

#|
|| **Type** | **Description** ||
|| `ONIMBOTV2MESSAGEADD` | New message to the bot ||
|| `ONIMBOTV2MESSAGEUPDATE` | Message edited ||
|| `ONIMBOTV2MESSAGEDELETE` | Message deleted ||
|| `ONIMBOTV2JOINCHAT` | Bot added to chat ||
|| `ONIMBOTV2DELETE` | Bot removed ||
|| `ONIMBOTV2CONTEXTGET` | Context of chat call passed ||
|| `ONIMBOTV2COMMANDADD` | Slash command invoked ||
|| `ONIMBOTV2REACTIONCHANGE` | Reaction changed ||
|#

Events are addressed to a specific bot — either by mentioning `@bot` or in a private chat. For bots of type `personal` and `supervisor`, all events in chats where the bot is present are delivered.

Detailed description of the data format for each event: [Event Formats for imbot.v2](./events.md).

## Error Handling

HTTP Status: **400**, **403**

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
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` not specified. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` not specified ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot registered by another application ||
|| `SCOPE_ERROR` | Scope error | Missing scope `im` — required when using `withUserEvents: true` ||
|| `AUTH_ERROR` | Auth error | Unable to determine the current user ||
|| `USER_NOT_SUBSCRIBED` | User not subscribed | User not subscribed to events via [im.v2.Event.subscribe](../../im.v2/events/event-subscribe.md) ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Chatbots 2.0: Quick Start](../../quick-start.md)
- [Chatbots 2.0: Overview of Methods](../../index.md)
- [Event Formats for imbot.v2](./events.md)
- [Register a Bot imbot.v2.Bot.register](../bots/bot-register.md)
- [Send Message imbot.v2.Chat.Message.send](../messages/chat-message-send.md)
- [Get User Events im.v2.Event.get](../../im.v2/events/event-get.md)
- [Subscribe to Events im.v2.Event.subscribe](../../im.v2/events/event-subscribe.md)
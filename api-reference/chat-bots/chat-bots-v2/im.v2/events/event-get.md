# Get User Events im.v2.Event.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute the method: authorized user

The method `im.v2.Event.get` retrieves events for the current user in polling mode.

{% note info "" %}

To receive events, the user must be previously subscribed via [im.v2.Event.subscribe](./event-subscribe.md). Without a subscription, events are not recorded, and the method will return an empty array.

{% endnote %}

The method uses confirmation via `offset`: pass the `nextOffset` from the previous response to get the next batch of events.

## Method Parameters

{% include [Parameter Note](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **offset**
[`integer`](../../../../data-types.md) | Confirms processed events and returns events starting from this value. Pass the `nextOffset` from the previous response ||
|| **limit**
[`integer`](../../../../data-types.md) | Maximum number of events returned (1–1000). Default is `100` ||
|#

## Code Examples

{% include [Examples Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"offset":2000,"limit":50}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.v2.Event.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"offset":2000,"limit":50,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.v2.Event.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.v2.Event.get', {
        offset: 2000,
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
                'im.v2.Event.get',
                [
                    'offset' => 2000,
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
        'im.v2.Event.get',
        {
            offset: 2000,
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
        'im.v2.Event.get',
        [
            'offset' => 2000,
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
                "eventId": 2001,
                "type": "ONIMV2MESSAGEADD",
                "date": "2025-01-15T10:30:00+02:00",
                "data": {}
            }
        ],
        "nextOffset": 2002,
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
[`string`](../../../../data-types.md) | Event type. List of types described [below](#event-types) ||
|| **result.events[].date**
[`datetime`](../../../../data-types.md) | Date and time of the event ||
|| **result.events[].data**
[`object`](../../../../data-types.md) | Event data. Format depends on the event type: [event description](./events.md) ||
|| **result.nextOffset**
[`integer`](../../../../data-types.md) | Offset for the next request. Pass in the `offset` parameter in the next call ||
|| **result.hasMore**
[`boolean`](../../../../data-types.md) | `true` if there are more unprocessed events ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Event Types {#event-types}

#|
|| **Type** | **Description** ||
|| `ONIMV2MESSAGEADD` | New message in chat ||
|| `ONIMV2MESSAGEUPDATE` | Message edited ||
|| `ONIMV2MESSAGEDELETE` | Message deleted ||
|| `ONIMV2REACTIONCHANGE` | Reaction to the message changed ||
|| `ONIMV2JOINCHAT` | New participant in the chat ||
|#

Detailed description of the data format for each event: [{#T}](./events.md).

## Exclusivity of Event Retrieval

Events for a specific user can only be received by **one application**. If multiple applications subscribe to the same user, the call with `offset` confirms and removes records for all — applications will "steal" events from each other.

This limitation is intentional: it is assumed that one agent should respond instantly to events. Multiple agents processing the same events for one user lead to duplicate and conflicting responses.

If multiple independent handlers are needed — use different user contexts or webhook subscriptions.

## Error Handling

The method does not generate specific errors. If the user is not subscribed via [im.v2.Event.subscribe](./event-subscribe.md), the method will return an empty array `events`.

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log imbot.v2](../../change-log.md)
- [{#T}](./event-subscribe.md)
- [{#T}](./event-unsubscribe.md)
- [{#T}](./events.md)
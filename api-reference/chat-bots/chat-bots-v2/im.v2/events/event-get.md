# Retrieve User Events im.v2.Event.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute the method: authorized user

The method `im.v2.Event.get` returns messenger events for the current user, such as new messages and reactions. The application itself calls the method periodically and collects the accumulated events — this is polling mode.

{% note info "" %}

First, subscribe the user to events with the [im.v2.Event.subscribe](./event-subscribe.md) method. Without a subscription, events are not recorded, and the method returns an empty `result.events` array.

{% endnote %}

Recorded events are stored for 24 hours. Bitrix24 deletes older events even if the application has not received them.

After processing the events, pass the `nextOffset` value in `offset`: the method removes them from the queue and returns the next batch.

If several applications subscribe the same user, any of them deletes the events for all of them by calling the method with `offset`: the user has a single queue. For details, see the [Limits and Restrictions](./index.md#limits) overview.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **offset**
[`integer`](../../../../data-types.md) | Pass `nextOffset` from the previous response. The method returns events with IDs starting from `offset` and removes events with lower IDs from the queue as confirmed. Without this parameter, the method confirms nothing. Default is `0`; a negative value is treated as zero.

If you pass a value greater than `nextOffset`, the method also deletes events you have not received yet ||
|| **limit**
[`integer`](../../../../data-types.md) | Maximum number of events returned (1–1000). Default is `100`. The method replaces an out-of-range value with the nearest limit ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

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
        method: 'im.v2.Event.get',
        params: {
          offset: 2000,
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
            method: 'im.v2.Event.get',
            params: {
              offset: 2000,
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
        bitrix_response = client.im.v2.event.get(
            offset=2000,
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.v2.Event.get", b24.Params{
    	"offset": 2000,
    	"limit":  50,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("im.v2.Event.get: %w", err)
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
                "eventId": 2001,
                "type": "ONIMV2MESSAGEADD",
                "date": "2024-10-11T09:58:12+02:00",
                "data": {
                    "message": {
                        "id": 189,
                        "chatId": 19,
                        "authorId": 129,
                        "date": "2024-10-11T09:58:12+02:00",
                        "text": "Good afternoon! When will the report be ready?",
                        "isSystem": false,
                        "params": []
                    },
                    "chat": {
                        "id": 19,
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
                    "additionalMessages": []
                }
            }
        ],
        "nextOffset": 2002,
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

The `message`, `chat`, and `user` objects in the example are shortened. For the full set of fields, see the [ONIMV2MESSAGEADD](./events.md#onimv2messageadd) event description.

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
[`string`](../../../../data-types.md) | Event type. List of types described [below](#event-types) ||
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
|| [ONIMV2MESSAGEADD](./events.md#onimv2messageadd) | New message in a chat ||
|| [ONIMV2MESSAGEUPDATE](./events.md#onimv2messageupdate) | Message edited ||
|| [ONIMV2MESSAGEDELETE](./events.md#onimv2messagedelete) | Message deleted ||
|| [ONIMV2REACTIONCHANGE](./events.md#onimv2reactionchange) | Reaction to message changed ||
|| [ONIMV2JOINCHAT](./events.md#onimv2joinchat) | New participant in a chat ||
|#

Detailed description of the data format for each event: [{#T}](./events.md).

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid value {abc} to match with parameter {offset}. Should be value of type int."
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | Invalid value {abc} to match with parameter {offset}. Should be value of type int. | A non-numeric value was passed in `offset` or `limit` ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API imbot.v2 Change Log](../../change-log.md)
- [im.v2 Events](./index.md)
- [{#T}](./event-subscribe.md)
- [{#T}](./event-unsubscribe.md)
- [{#T}](./events.md)
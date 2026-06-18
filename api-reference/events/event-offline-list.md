# Get a List of Offline Events event.offline.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Who can execute the method: any user

The `event.offline.list` method is used to read the current queue without altering its state, unlike [event.offline.get](./event-offline-get.md). The availability of offline events can be checked using the [feature.get](../common/system/feature-get.md) method.

This method does not mark events as processed and does not generate a `process_id`. In the records, the `PROCESS_ID` field remains empty until events are reserved by calling [event.offline.get](./event-offline-get.md) with the `clear=0` parameter.

The method operates only within the context of application authorization [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **filter**
[`array`](../data-types.md) | Record filter. By default, all records are returned without filtering. Filtering is supported by fields: `ID`, `TIMESTAMP_X`, `EVENT_NAME`, `MESSAGE_ID`, `PROCESS_ID`, `ERROR` with standard operations like `=`, `>`, `<`, `<=`, etc. ||
|| **order**
[`array`](../data-types.md) | Record sorting. Sorting is supported by the same fields as in the filter, and the input is an array in the format `[field=>ASC|DESC]`. By default — `[ID:ASC]` ||
|| **start**
[`integer`](../data-types.md) | This parameter is used to manage pagination.

The page size for results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number. ||
|| **auth_connector**
[`string`](../data-types.md) | Source key. The queue of offline events is divided by sources. Pass the same `auth_connector` value as when subscribing with the [event.bind](./event-bind.md) method; otherwise, the method will return only events without a source. This parameter is available on the Professional plan and above. ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "filter": {
            "ERROR": 0
        },
        "order": {
            "ID": "DESC"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/event.offline.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each OfflineEventItem returned in result[]
    type OfflineEventItem = {
      ID: string
      TIMESTAMP_X: ISODate
      EVENT_NAME: string
      EVENT_DATA: unknown
      EVENT_ADDITIONAL: unknown
      MESSAGE_ID: string
      PROCESS_ID: string
      ERROR: string
    }

    try {
      // event.offline.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<OfflineEventItem[]>({
        method: 'event.offline.list',
        params: {
          filter: {
            ERROR: 0,
          },
          order: {
            ID: 'DESC',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Offline events:', result.length, result)
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
      async function fetchOfflineEvents() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // event.offline.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'event.offline.list',
            params: {
              filter: {
                ERROR: 0,
              },
              order: {
                ID: 'DESC',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Offline events:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchOfflineEvents)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'event.offline.list',
                [
                    'filter' => [
                        'ERROR' => 0,
                    ],
                    'order' => [
                        'ID' => 'DESC',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching offline events: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "event.offline.list",
        {
            "filter": {
                "ERROR": 0
            },
            "order": {
                "ID": "DESC"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.offline.list',
        [
            'filter' => [
                'ERROR' => 0
            ],
            'order' => [
                'ID' => 'DESC'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "2",
            "TIMESTAMP_X": "2024-07-18T12:32:31+02:00",
            "EVENT_NAME": "ONCRMCOMPANYADD",
            "EVENT_DATA": false,
            "EVENT_ADDITIONAL": false,
            "MESSAGE_ID": "2",
            "PROCESS_ID": "",
            "ERROR": "0"
        },
        {
            "ID": "1",
            "TIMESTAMP_X": "2024-07-18T12:32:31+02:00",
            "EVENT_NAME": "ONCRMLEADADD",
            "EVENT_DATA": false,
            "EVENT_ADDITIONAL": false,
            "MESSAGE_ID": "1",
            "PROCESS_ID": "",
            "ERROR": "0"
        }
    ],
    "total": 2,
    "time": {
        "start": 1721299537.90267,
        "finish": 1721299538.02201,
        "duration": 0.11934018135070801,
        "processing": 0.0029511451721191406,
        "date_start": "2024-07-18T12:45:37+02:00",
        "date_finish": "2024-07-18T12:45:38+02:00",
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response ||
|| **total**
[`integer`](../data-types.md) | Total number of records found ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./events.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
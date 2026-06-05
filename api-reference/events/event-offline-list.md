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

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.
    
    try {
      const response = await $b24.callListMethod(
        'event.offline.list',
        {
          "filter": {
            "ERROR": 0
          },
          "order": {
            "ID": "DESC"
          }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use for large volumes of data for efficient memory consumption.
    
    try {
      const generator = $b24.fetchListMethod('event.offline.list', { "filter": { "ERROR": 0 }, "order": { "ID": "DESC" } }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('event.offline.list', { "filter": { "ERROR": 0 }, "order": { "ID": "DESC" } }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
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
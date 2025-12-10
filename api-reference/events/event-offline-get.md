# Get a List of Offline Events with `event.offline.get`

> Who can execute the method: any user

The method `event.offline.get` returns the first queued offline events to the application according to the filter settings. The availability of offline events can be checked using the [feature.get](../common/system/feature-get.md) method.

This method works only in the context of [application](../../settings/app-installation/index.md) authorization.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`array`](../data-types.md) | Record filter. By default, all records are returned without filtering. Filtering is supported by fields: `ID`, `TIMESTAMP_X`, `EVENT_NAME`, `MESSAGE_ID` with standard operations like `=`, `>`, `<`, `<=`, and so on.

Important: the operation type is placed before the field name in the filter ||
|| **order**
[`array`](../data-types.md) | Record sorting. Sorting is supported by the same fields as in the filter, and an array of the form `[field=>ASC|DESC]` is accepted. By default — [TIMESTAMP_X:ASC] ||
|| **limit**
[`integer`](../data-types.md) | Number of records to select. Default is 50 ||
|#

### Additional Parameters

#|
|| **Name**
`type` | **Description** ||
|| **clear**
[`integer`](../data-types.md) | Values: `0|1` — whether to delete the selected records. Default is `1` ||
|| **process_id**
[`string`](../data-types.md) | Process identifier. Used if you need to re-select any unprocessed records from the current process ||
|| **auth_connector**
[`string`](../data-types.md) | Source key. Used if the `auth_connector` value was specified in the [event.bind](./event-bind.md) method ||
|| **error**
[`integer`](../data-types.md) | Values: `0|1` — whether to return erroneous records. Default is `0` ||
|#

{% note info %}

The method supports multithreaded parsing. This means that multiple parallel requests to /rest/event.offline.get are allowed (subject to limits on the number of requests per unit of time), and each will receive different sets of records.

{% endnote %}

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"=MESSAGE_ID":1,"=EVENT_NAME":"ONCRMLEADADD",">=ID":1},"auth_connector":"BxTest","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/event.offline.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'event.offline.get',
            {
                filter: {
                    '=MESSAGE_ID': 1,
                    '=EVENT_NAME': 'ONCRMLEADADD',
                    '>=ID': 1
                },
                auth_connector: 'BxTest'
            }
        );
        
        const result = response.getData().result;
        console.log('Result:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'event.offline.get',
                [
                    'filter' => [
                        '=MESSAGE_ID' => 1,
                        '=EVENT_NAME' => 'ONCRMLEADADD',
                        '>=ID' => 1
                    ],
                    'auth_connector' => 'BxTest'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "event.offline.get",
        {
            "filter": {
                "=MESSAGE_ID": 1,
                "=EVENT_NAME": "ONCRMLEADADD",
                ">=ID": 1
            },
            "auth_connector": "BxTest"
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
        'event.offline.get',
        [
            'filter' => [
                '=MESSAGE_ID' => 1,
                '=EVENT_NAME' => 'ONCRMLEADADD',
                '>=ID' => 1
            ],
            'auth_connector' => 'BxTest'
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
    "result": {
        "process_id": null,
        "events": [
            {
                "ID": "1",
                "TIMESTAMP_X": "2024-07-18T12:32:31+02:00",
                "EVENT_NAME": "ONCRMLEADADD",
                "EVENT_DATA": false,
                "EVENT_ADDITIONAL": false,
                "MESSAGE_ID": "1"
            }
        ]
    },
    "time": {
        "start": 1721299720.388504,
        "finish": 1721299720.509809,
        "duration": 0.12130498886108398,
        "processing": 0.008239030838012695,
        "date_start": "2024-07-18T12:48:40+02:00",
        "date_finish": "2024-07-18T12:48:40+02:00",
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
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
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
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
# Register Errors for Processing Offline Events event.offline.error

> Who can execute the method: any user

The method `event.offline.error` saves a record in the database with an error mark when using offline events. The availability of offline events can be checked through the method [feature.get](../common/system/feature-get.md).

The method works only in the context of authorizing the [application](../app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **process_id***
[`string`](../data-types.md) | Identifier of the process handling the records ||
|| **message_id**
[`array`](../data-types.md) | Array of values for the `MESSAGE_ID` field of the records to be marked as erroneous ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "process_id": "yh3gu929sf0d32lsfysqas2y1hlpp09q",
        "message_id": [2],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/event.offline.error
    ```

- JS

    ```js
    BX24.callMethod(
        "event.offline.error",
        {
            "process_id": "yh3gu929sf0d32lsfysqas2y1hlpp09q",
            "message_id": [2]
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.offline.error',
        [
            'process_id' => 'yh3gu929sf0d32lsfysqas2y1hlpp09q',
            'message_id' => [2]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1721300231.498173,
        "finish": 1721300231.596196,
        "duration": 0.0980229377746582,
        "processing": 0.0019490718841552734,
        "date_start": "2024-07-18T12:57:11+02:00",
        "date_finish": "2024-07-18T12:57:11+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Success of execution ||
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
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./on-offline-event.md)
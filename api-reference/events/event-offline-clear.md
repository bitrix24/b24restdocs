# Clear Records in the Offline Events Queue event.offline.clear

> Who can execute the method: any user

The method `event.offline.clear` clears records in the offline events queue. The availability of offline events can be checked using the [feature.get](../common/system/feature-get.md) method.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **process_id***
[`string`](../data-types.md) | Identifier of the process that handles the records ||
|| **id**
[`array`](../data-types.md) | Array of record identifiers to be cleared. By default, all records marked with the provided `process_id` will be cleared ||
|| **message_id**
[`array`](../data-types.md) | Array of values for the `MESSAGE_ID` field of the records to be cleared. Ignored if the `id` parameter is specified. By default, all records marked with the provided `process_id` will be cleared ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "process_id": "yh3gu929sf0d32lsfysqas2y1hlpp09q",
        "id": [2]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/event.offline.clear
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "process_id": "yh3gu929sf0d32lsfysqas2y1hlpp09q",
        "id": [2],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/event.offline.clear
    ```

- JS

    ```js
    BX24.callMethod(
        "event.offline.clear",
        {
            "process_id": "yh3gu929sf0d32lsfysqas2y1hlpp09q",
            "id": [2]
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
        'event.offline.clear',
        [
            'process_id' => 'yh3gu929sf0d32lsfysqas2y1hlpp09q',
            'id' => [2]
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
        "start": 1721300421.210707,
        "finish": 1721300421.331026,
        "duration": 0.12031912803649902,
        "processing": 0.0022459030151367188,
        "date_start": "2024-07-18T13:00:21+02:00",
        "date_finish": "2024-07-18T13:00:21+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Success of the execution ||
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
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
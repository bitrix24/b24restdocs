# Delete Measurement Unit catalog.measure.delete

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a measurement unit.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_measure.id`](../data-types.md#catalog_measure) | Identifier of the measurement unit ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.measure.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.measure.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.measure.delete', 
        { 
    id: 6 
    }, 
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.measure.delete',
        [
            'id' => 6
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
    "result": true,
    "time": {
        "start": 1716558215.93495,
        "finish": 1716558216.20731,
        "duration": 0.272363901138306,
        "processing": 0.028817892074585,
        "date_start": "2024-05-24T15:43:35+02:00",
        "date_finish": "2024-05-24T15:43:36+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the measurement unit deletion ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 200040300020,
    "error_description": "Access Denied",
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to delete
||
|| `200600000020` | Measurement unit with this identifier does not exist 
||
|| `100` | Parameter `id` is not specified
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-measure-add.md)
- [{#T}](./catalog-measure-update.md)
- [{#T}](./catalog-measure-get.md)
- [{#T}](./catalog-measure-list.md)
- [{#T}](./catalog-measure-get-fields.md)
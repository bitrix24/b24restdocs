# Get Unit of Measurement Ratio Field Values catalog.ratio.get

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns the field values of the unit of measurement ratio by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_ratio.id`](../data-types.md#catalog_ratio) | Identifier of the unit of measurement ratio.

To obtain the identifiers of unit of measurement ratios, use the [catalog.ratio.list](./catalog-ratio-list.md) method.
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.ratio.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.ratio.get
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.ratio.get', {
            id: 1,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.ratio.get',
        [
            'id' => 1
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
        "ratio": {
            "id": 1,
            "isDefault": "Y",
            "productId": 1,
            "ratio": 1
        }
    },
    "time": {
        "start": 1729601856.749788,
        "finish": 1729601857.530307,
        "duration": 0.7805190086364746,
        "processing": 0.07734394073486328,
        "date_start": "2024-10-22T15:57:36+02:00",
        "date_finish": "2024-10-22T15:57:37+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **ratio**
[`catalog_ratio`](../data-types.md#catalog_ratio) | Object containing information about the unit of measurement ratio ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{	
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to view the unit of measurement ratio
||
|| `100` | Parameter `id` is missing
||
|| `0` | The unit of measurement ratio does not exist
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-ratio-list.md)
- [{#T}](./catalog-ratio-get-fields.md)
# Delete Service catalog.product.service.delete

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a service.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_service.id`](../../data-types.md#catalog_product_service) | Identifier of the service.

To obtain service identifiers, you need to use [catalog.product.service.list](./catalog-product-service-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1264}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1264,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.service.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.product.service.delete',
        {
            id: 1264,
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
        'catalog.product.service.delete',
        [
            'id' => 1264
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
        "start": 1718362658.676929,
        "finish": 1718362659.517447,
        "duration": 0.8405179977416992,
        "processing": 0.39598703384399414,
        "date_start": "2024-06-14T13:57:38+03:00",
        "date_finish": "2024-06-14T13:57:39+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the service deletion ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{	
    "error":200040300040,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300040` | Insufficient permissions to delete the service
|| 
|| `200040300040` | Insufficient permissions to delete the information block
|| 
|| `200040300010` | Insufficient permissions to view the trade catalog
|| 
|| `200040300000` | Information block not found
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-add.md)
- [{#T}](./catalog-product-service-update.md)
- [{#T}](./catalog-product-service-get.md)
- [{#T}](./catalog-product-service-list.md)
- [{#T}](./catalog-product-service-download.md)
- [{#T}](./catalog-product-service-get-fields-by-filter.md)
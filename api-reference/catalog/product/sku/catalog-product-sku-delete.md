# Delete Parent Product catalog.product.sku.delete

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes the parent product.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_sku.id`](../../data-types.md#catalog_product_sku) | Identifier of the parent product.

To obtain the identifiers of parent products, you need to use [catalog.product.sku.list](./catalog-product-sku-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1288}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1288,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.sku.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.product.sku.delete',
        {
            id: 1288,
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
        'catalog.product.sku.delete',
        [
            'id' => 1288
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
        "start": 1718630898.795719,
        "finish": 1718630899.598307,
        "duration": 0.8025879859924316,
        "processing": 0.35277414321899414,
        "date_start": "2024-06-17T16:28:18+03:00",
        "date_finish": "2024-06-17T16:28:19+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the parent product deletion ||
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
|| `200040300040` | Insufficient permissions to delete the parent product
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

- [{#T}](./catalog-product-sku-add.md)
- [{#T}](./catalog-product-sku-update.md)
- [{#T}](./catalog-product-sku-get.md)
- [{#T}](./catalog-product-sku-list.md)
- [{#T}](./catalog-product-sku-download.md)
- [{#T}](./catalog-product-sku-get-fields-by-filter.md)
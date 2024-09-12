# Get Head Product Field Values catalog.product.sku.get

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns the field values of the head product by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_sku.id`](../../data-types.md#catalog_product_sku) | Identifier of the head product.

To obtain the identifiers of head products, you need to use [catalog.product.sku.list](./catalog-product-sku-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1289}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1289,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.sku.get
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.product.sku.get', {
            'id': 1289
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
        'catalog.product.sku.get',
        [
            'id' => 1289
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
        "sku": {
            "active": "Y",
            "available": "N",
            "bundle": "N",
            "canBuyZero": "Y",
            "code": "product_sku",
            "createdBy": 1,
            "dateActiveFrom": "2024-05-28T10:00:00+03:00",
            "dateActiveTo": "2024-05-29T10:00:00+03:00",
            "dateCreate": "2024-05-27T10:00:00+03:00",
            "detailPicture": {
                "id": "6552",
                "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6552\u0026fields%5BproductId%5D=1289",
                "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6552\u0026fields%5BproductId%5D=1289"
            },
            "detailText": null,
            "detailTextType": "text",
            "height": 100,
            "iblockId": 23,
            "iblockSectionId": 47,
            "id": 1289,
            "length": 100,
            "measure": 5,
            "modifiedBy": 1,
            "name": "Head Product",
            "previewPicture": {
                "id": "6551",
                "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6551\u0026fields%5BproductId%5D=1289",
                "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6551\u0026fields%5BproductId%5D=1289"
            },
            "previewText": null,
            "previewTextType": "text",
            "property258": {
                "value": "test",
                "valueId": "9877"
            },
            "property259": [
                {
                    "value": "test1",
                    "valueId": "9878"
                },
                {
                    "value": "test2",
                    "valueId": "9879"
                }
            ],
            "purchasingCurrency": "USD",
            "purchasingPrice": "1000.000000",
            "quantity": 10,
            "sort": 100,
            "subscribe": "Y",
            "timestampX": "2024-06-17T16:03:20+03:00",
            "type": 6,
            "vatId": 1,
            "vatIncluded": "Y",
            "weight": 100,
            "width": 100,
            "xmlId": "1243"
        }
    },
    "time": {
        "start": 1718636890.413679,
        "finish": 1718636891.096817,
        "duration": 0.6831381320953369,
        "processing": 0.27536606788635254,
        "date_start": "2024-06-17T18:08:10+03:00",
        "date_finish": "2024-06-17T18:08:11+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **sku**
[`catalog_product_sku`](../../data-types.md#catalog_product_sku) | Object containing information about the head product ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300000` | The information block with the specified identifier does not exist
|| 
|| `200040300040` | Insufficient rights to read the information block element
|| 
|| `200040300010` | Insufficient rights to read the trade catalog
|| 
|| `100` | The `id` parameter is not specified
|| 
|| `0` | The head product does not exist
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-sku-add.md)
- [{#T}](./catalog-product-sku-update.md)
- [{#T}](./catalog-product-sku-list.md)
- [{#T}](./catalog-product-sku-download.md)
- [{#T}](./catalog-product-sku-delete.md)
- [{#T}](./catalog-product-sku-get-fields-by-filter.md)
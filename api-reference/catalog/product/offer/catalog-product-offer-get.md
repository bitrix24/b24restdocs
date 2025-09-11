# Get Product Variation Field Values catalog.product.offer.get

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns the field values of a product variation by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_offer.id`](../../data-types.md#catalog_product_offer) | Identifier of the product variation.

To obtain the identifiers of product variations, you need to use [catalog.product.offer.list](./catalog-product-offer-list.md)
 ||
|#

{% note warning "Working with Product Price" %}

To get the prices of variations, use the methods [catalog.price.*](../../price/index.md).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1286}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1286,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.product.offer.get', {
    			'id': 1286
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.offer.get',
                [
                    'id' => 1286
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting product offer: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.offer.get', {
            'id': 1286
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.offer.get',
        [
            'id' => 1286
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
        "offer": {
            "active": "Y",
            "available": "Y",
            "bundle": "N",
            "canBuyZero": "Y",
            "code": "Product",
            "createdBy": 1,
            "dateActiveFrom": "2024-05-28T10:00:00+02:00",
            "dateActiveTo": "2024-05-29T10:00:00+02:00",
            "dateCreate": "2024-05-27T10:00:00+02:00",
            "detailPicture": {
                "id": "6538",
                "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6538\u0026fields%5BproductId%5D=1286",
                "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6538\u0026fields%5BproductId%5D=1286"
            },
            "detailText": null,
            "detailTextType": "text",
            "height": 100,
            "iblockId": 24,
            "iblockSectionId": null,
            "id": 1286,
            "length": 100,
            "measure": 5,
            "modifiedBy": 1,
            "name": "Product Variation",
            "parentId": {
                "value": "1275",
                "valueId": "9867"
            },
            "previewPicture": {
                "id": "6537",
                "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6537\u0026fields%5BproductId%5D=1286",
                "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6537\u0026fields%5BproductId%5D=1286"
            },
            "previewText": null,
            "previewTextType": "text",
            "property261": {
                "value": "test",
                "valueId": "9868"
            },
            "property262": [
                {
                    "value": "test1",
                    "valueId": "9869"
                },
                {
                    "value": "test2",
                    "valueId": "9870"
                }
            ],
            "purchasingCurrency": "EUR",
            "purchasingPrice": "1000.000000",
            "quantity": 10,
            "quantityReserved": 1,
            "quantityTrace": "Y",
            "sort": 100,
            "subscribe": "Y",
            "timestampX": "2024-06-17T12:29:59+02:00",
            "type": 4,
            "vatId": 1,
            "vatIncluded": "Y",
            "weight": 100,
            "width": 100,
            "xmlId": "1286"
        }
    },
    "time": {
        "start": 1718623874.271071,
        "finish": 1718623874.871713,
        "duration": 0.6006419658660889,
        "processing": 0.1868131160736084,
        "date_start": "2024-06-17T14:31:14+02:00",
        "date_finish": "2024-06-17T14:31:14+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **offer**
[`catalog_product_offer`](../../data-types.md#catalog_product_offer) | Object with information about the product variation ||
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
|| `0` | The product variation does not exist
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add.md)
- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-list.md)
- [{#T}](./catalog-product-offer-download.md)
- [{#T}](./catalog-product-offer-delete.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)
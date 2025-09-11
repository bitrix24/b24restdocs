# Get Service Field Values catalog.product.service.get

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns the field values of the service by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_service.id`](../../data-types.md#catalog_product_service) | Identifier of the service.

To obtain service identifiers, use [catalog.product.service.list](./catalog-product-service-list.md) 
||
|#

{% note warning "Working with Service Prices" %}

To get service prices, use the methods [catalog.price.*](../../price/index.md).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1265}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1265,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.service.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.product.service.get', {
    			id: 1265
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
                'catalog.product.service.get',
                [
                    'id' => 1265
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
        echo 'Error getting product service: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.service.get', {
            id: 1265
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
        'catalog.product.service.get',
        [
            'id' => 1265
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
        "service": {
            "active": "Y",
            "available": "N",
            "bundle": "N",
            "code": "service",
            "createdBy": 1,
            "dateActiveFrom": "2024-05-28T10:00:00+02:00",
            "dateActiveTo": "2024-05-29T10:00:00+02:00",
            "dateCreate": "2024-05-27T10:00:00+02:00",
            "detailPicture": {
                "id": "6497",
                "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6497\u0026fields%5BproductId%5D=1265",
                "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6497\u0026fields%5BproductId%5D=1265"
            },
            "detailText": null,
            "detailTextType": "text",
            "iblockId": 23,
            "iblockSectionId": 47,
            "id": 1265,
            "modifiedBy": 1,
            "name": "Service",
            "previewPicture": {
                "id": "6496",
                "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6496\u0026fields%5BproductId%5D=1265",
                "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6496\u0026fields%5BproductId%5D=1265"
            },
            "previewText": null,
            "previewTextType": "text",
            "property258": {
                "value": "test",
                "valueId": "9809"
            },
            "property259": [
                {
                    "value": "test1",
                    "valueId": "9810"
                },
                {
                    "value": "test2",
                    "valueId": "9811"
                }
            ],
            "sort": 100,
            "timestampX": "2024-06-14T11:59:04+02:00",
            "type": 7,
            "vatId": 1,
            "vatIncluded": "Y",
            "xmlId": "1265"
        }
    },
    "time": {
        "start": 1718363239.355821,
        "finish": 1718363240.027938,
        "duration": 0.6721169948577881,
        "processing": 0.2661628723144531,
        "date_start": "2024-06-14T14:07:19+02:00",
        "date_finish": "2024-06-14T14:07:20+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **service**
[`catalog_product_service`](../../data-types.md#catalog_product_service) | Object containing service information ||
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
|| `0` | The service does not exist
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-add.md)
- [{#T}](./catalog-product-service-update.md)
- [{#T}](./catalog-product-service-list.md)
- [{#T}](./catalog-product-service-download.md)
- [{#T}](./catalog-product-service-delete.md)
- [{#T}](./catalog-product-service-get-fields-by-filter.md)
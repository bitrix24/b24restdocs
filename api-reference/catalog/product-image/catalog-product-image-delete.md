# Delete Image from Product catalog.productImage.delete

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method removes an image from a product, parent product, variation, or service.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **productId***
[`catalog_product.id`](../data-types.md#catalog_product)\|
[`catalog_product_sku.id`](../data-types.md#catalog_product_sku)\|
[`catalog_product_offer.id`](../data-types.md#catalog_product_offer)\|
[`catalog_product_service.id`](../data-types.md#catalog_product_service) | Identifier of the product, parent product, variation, or service.

To obtain existing identifiers, use the following methods:
- for products — [catalog.product.list](../product/catalog-product-list.md)
- for parent products — [catalog.product.sku.list](../product/sku/catalog-product-sku-list.md)
- for product variations — [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md)
- for services — [catalog.product.service.list](../product/service/catalog-product-service-list.md)
||
|| **id***
[`catalog_product_image.id`](../data-types.md#catalog_product_image) | Identifier of the image ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"productId":1,"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productImage.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"productId":1,"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.productImage.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.productImage.delete',
    		{
    			productId: 1,
    			id: 1
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
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
                'catalog.productImage.delete',
                [
                    'productId' => 1,
                    'id'        => 1
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting product image: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productImage.delete',
        {
            productId: 1,
            id: 1
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.productImage.delete',
        [
            'productId' => 1,
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
    "result": true,
    "time": {
        "start": 1729001588.54184,
        "finish": 1729001589.40018,
        "duration": 0.8583400249481201,
        "processing": 0.4529080390930176,
        "date_start": "2024-10-15T17:13:08+02:00",
        "date_finish": "2024-10-15T17:13:09+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the image deletion ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{	
    "error":200040300020,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to modify the trade catalog
||
|| `200040300020` | Insufficient permissions to modify the product
||
|| `100` | Parameter `productId` is missing or empty
||
|| `100` | Parameter `id` is missing or empty
|| 
|| `0` | Product with the specified identifier not found
|| 
|| `0` | Image with the specified identifier not found
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-product-image-add.md)
- [{#T}](./catalog-product-image-get.md)
- [{#T}](./catalog-product-image-list.md)
- [{#T}](./catalog-product-image-get-fields.md)
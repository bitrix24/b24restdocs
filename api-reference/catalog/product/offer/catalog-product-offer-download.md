# Download Product Variation Files catalog.product.offer.download

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method downloads product variation files based on the provided parameters.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values for downloading product variation files ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fileId***
[`integer`](../../../data-types.md) | Identifier of the registered file.

To obtain file identifiers for product variations, use [catalog.product.offer.get](./catalog-product-offer-get.md) or [catalog.product.offer.list](./catalog-product-offer-list.md)
||
|| **productId***
[`catalog_product_offer.id`](../../data-types.md#catalog_product_offer) | Identifier of the product variation.

To obtain identifiers for product variations, use [catalog.product.offer.list](./catalog-product-offer-list.md)
||
|| **fieldName***
[`string`](../../../data-types.md) | The name of the field (property or field of the information block element) where the file is stored. Possible values:
- `DETAIL_PICTURE` — detailed image
- `PREVIEW_PICTURE` — preview image
- `PROPERTY_N` — property, where `N` is the property identifier or code

To obtain existing identifiers or codes of properties for product variations, use [catalog.productProperty.list](../../product-property/catalog-product-property-list.md)
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":6538,"productId":1286,"fieldName":"detailPicture"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fileId":6538,"productId":1286,"fieldName":"detailPicture"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.download
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.product.offer.download',
    		{
    			fields: {
    				fileId: 6538,
    				productId: 1286,
    				fieldName: 'detailPicture',
    			}
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
                'catalog.product.offer.download',
                [
                    'fields' => [
                        'fileId'     => 6538,
                        'productId'  => 1286,
                        'fieldName'  => 'detailPicture',
                    ],
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
        echo 'Error downloading product offer: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.offer.download',
        {
            fields: {
                fileId: 6538,
                productId: 1286,
                fieldName: 'detailPicture',
            }
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
        'catalog.product.offer.download',
        [
            'fields' => [
                'fileId' => 6538,
                'productId' => 1286,
                'fieldName' => 'detailPicture'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

A file is returned based on the provided parameters.

### Returned Data

A file is returned based on the provided parameters.

## Error Handling

HTTP status: **400**

```json
{	
    "error":0,
    "error_description":"Required fields: fileId"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the product catalog
|| 
|| `0` | The product variation with the specified identifier does not exist
|| 
|| `0` | The specified property does not exist or is not a file
|| 
|| `0` | The file with the specified identifier does not exist
|| 
|| `0` | Required fields are not provided
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add.md)
- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-get.md)
- [{#T}](./catalog-product-offer-list.md)
- [{#T}](./catalog-product-offer-delete.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)
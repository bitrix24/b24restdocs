# Delete Product catalog.product.delete

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method removes a product from the trade catalog.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`catalog_product.id`](../data-types.md#catalog_product)| Product identifier.

To obtain product identifiers, use the [catalog.product.list](./catalog-product-list.md)
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
    -d '{"id":1242}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1242,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.product.delete',
    		{
    			id: 1242,
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
        $productId = 123; // Replace with the actual product ID you want to delete
        $result = $serviceBuilder
            ->getCatalogScope()
            ->product()
            ->delete($productId);
        if ($result->isSuccess()) {
            print("Product with ID {$productId} was deleted successfully.");
        } else {
            print("Failed to delete product with ID {$productId}.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.delete',
        {
            id: 1242,
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
        'catalog.product.delete',
        [
            'id' => 1242
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
        "start": 1717578025.70057,
        "finish": 1717578176.028261,
        "duration": 150.32769083976746,
        "processing": 149.8356819152832,
        "date_start": "2024-06-05T12:00:25+02:00",
        "date_finish": "2024-06-05T12:02:56+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of product deletion ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{	
   "error":200040300040,
   "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300040` | Insufficient permissions to delete the product ||
|| `200040300040` | Insufficient permissions to delete the information block ||
|| `200040300010` | Insufficient permissions to view the trade catalog ||
|| `200040300000` | Information block not found ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-product-add.md)
- [{#T}](./catalog-product-update.md)
- [{#T}](./catalog-product-get.md)
- [{#T}](./catalog-product-list.md)
- [{#T}](./catalog-product-download.md)
- [{#T}](./catalog-product-get-fields-by-filter.md)
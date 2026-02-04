# Create/Update Product Items in crm.quote.productrows.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing (should include three examples - curl, js, php)
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method Development Stopped" %}

The method `crm.quote.productrows.set` continues to function, but there is a more relevant alternative [crm.item.productrow.*](../universal/product-rows/index.md).

{% endnote %}

The method `crm.quote.productrows.set` sets (creates or updates) the product items of the quote.

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the quote. ||
|| **rows**
[`unknown`](../../data-types.md) | Product items - an array of the form `array(array("field"=>"value"[, ...])[, ...])`, where "field" can take values returned by the method [crm.productrow.fields](../../crm/outdated/productrow-old/crm-productrow-fields.md). The product items of the quote that exist before the method call will be replaced with the new ones. After saving, the total amount of the quote will be recalculated. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.quote.productrows.set",
    		{
    			id: id,
    			rows:
    			[
    				{ "PRODUCT_ID": 1, "PRICE": 100.00, "QUANTITY": 2 },
    				{ "PRODUCT_ID": 2, "PRICE": 200.00, "QUANTITY": 1 }
    			]
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.info(result);
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    $id = readline("Enter ID");
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.productrows.set',
                [
                    'id'   => $id,
                    'rows' => [
                        ['PRODUCT_ID' => 1, 'PRICE' => 100.00, 'QUANTITY' => 2],
                        ['PRODUCT_ID' => 2, 'PRICE' => 200.00, 'QUANTITY' => 1],
                    ],
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
        echo 'Error setting quote product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.quote.productrows.set",
        {
            id: id,
            rows:
            [
                { "PRODUCT_ID": 1, "PRICE": 100.00, "QUANTITY": 2 },
                { "PRODUCT_ID": 2, "PRICE": 200.00, "QUANTITY": 1 }
            ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}
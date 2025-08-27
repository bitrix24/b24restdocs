# Add products to lead crm.lead.productrows.set

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (in other languages)
- success response is missing
- error response is missing
- link to the yet-to-be-created page is not provided

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.productrows.set` sets (creates or updates) the product items of a lead.

#|
|| **Parameter** | **Description** ||
|| **id** | Lead identifier. ||
|| **rows** | Product items - an array of the form `array(array("field"=>"value"[, ...])[, ...])`, where "field" can take values from the method [crm.productrow.fields](../outdated/productrow-old/crm-productrow-fields.md). The product items of the lead that exist before the method call will be replaced with the new ones. After saving, the lead's total will be recalculated. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.lead.productrows.set",
    		{
    			id: id,
    			rows:
    			[
    				{ "PRODUCT_ID": 689, "PRICE": 100.00, "QUANTITY": 2 },
    				{ "PRODUCT_ID": 690, "PRICE": 200.00, "QUANTITY": 1 }
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
                'crm.lead.productrows.set',
                [
                    'id'   => $id,
                    'rows' => [
                        ['PRODUCT_ID' => 689, 'PRICE' => 100.00, 'QUANTITY' => 2],
                        ['PRODUCT_ID' => 690, 'PRICE' => 200.00, 'QUANTITY' => 1]
                    ]
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
        echo 'Error setting lead product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.lead.productrows.set",
        {
            id: id,
            rows:
            [
                { "PRODUCT_ID": 689, "PRICE": 100.00, "QUANTITY": 2 },
                { "PRODUCT_ID": 690, "PRICE": 200.00, "QUANTITY": 1 }
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

{% include [Footnote about examples](../../../_includes/examples.md) %}
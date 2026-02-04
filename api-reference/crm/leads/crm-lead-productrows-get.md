# Get Lead Products crm.lead.productrows.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method Development Stopped" %}

The method `crm.lead.productrows.get` continues to function, but there is a more relevant alternative [crm.item.productrow.*](../universal/product-rows/index.md).

{% endnote %}

The method `crm.lead.productrows.get` returns the product rows of a lead.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Lead identifier. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.lead.productrows.get",
    		{ id: id }
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    $id = $_POST['id'];
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.lead.productrows.get',
                [
                    'id' => $id,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting lead product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.lead.productrows.get",
        { id: id },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}
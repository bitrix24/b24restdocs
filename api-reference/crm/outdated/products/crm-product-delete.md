# Delete product crm.product.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator, user with the "Allow to modify settings" access permission in CRM

{% note warning "Method development has been halted" %}

The method `crm.product.delete` is still operational, but there are more relevant alternatives [catalog.product.*](../../../catalog/product/index.md).

{% endnote %}

The method `crm.product.delete` removes a product.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Product identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_product_id"}' \ # Replace 'your_product_id' with the actual product ID
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_product_id"}' \ # Replace 'your_product_id' with the actual product ID
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.delete
    ```

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.product.delete",
    		{ id: id }
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
    try {
        $productId = 123; // Example product ID
        $result = $serviceBuilder->getCRMScope()->product()->delete($productId);
        
        if ($result->isSuccess()) {
            print("Item deleted successfully.");
        } else {
            print("Failed to delete item.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.product.delete",
        { id: id },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $id = 'your_product_id'; // Replace 'your_product_id' with the actual product ID

    $result = CRest::call(
        'crm.product.delete',
        [
            'id' => $id
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}
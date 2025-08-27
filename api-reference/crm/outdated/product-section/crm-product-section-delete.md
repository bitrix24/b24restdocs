# Delete Product Section crm.productsection.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.productsection.delete` removes a product catalog section.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the product section ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    id=$(prompt "Enter ID")

    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": '"$id"'
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.productsection.delete
    ```

- cURL (OAuth)

    ```curl
    id=$(prompt "Enter ID")

    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": '"$id"',
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/crm.productsection.delete
    ```

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.productsection.delete",
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
    $id = readline("Enter ID");
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.productsection.delete',
                [
                    'id' => $id,
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
        echo 'Error deleting product section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.productsection.delete",
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

    $id = readline("Enter ID: ");

    $result = CRest::call(
        'crm.productsection.delete',
        [
            'id' => $id
        ]
    );

    if (isset($result['error'])) {
        echo "Error: " . $result['error_description'] . "\n";
    } else {
        echo "Deleted section with ID " . $id . "\n";
    }
    ```

{% endlist %}
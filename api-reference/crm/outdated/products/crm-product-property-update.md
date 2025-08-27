# Update Product Property crm.product.property.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator, user with the "Allow changing settings" access permission in CRM

{% note warning "Method development halted" %}

The method `crm.product.property.update` is still operational, but there is a more current equivalent [catalog.productProperty.update](../../../catalog/product-property/catalog-product-property-update.md).

{% endnote %}

The method `crm.product.property.update` updates an existing product property.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the product property ||
|| **fields**
[`array`](../../../data-types.md) | Field values for updating the product property.

To find out the required format of the fields, execute the method [crm.product.property.fields](./crm-product-property-fields.md) and check the format of the incoming values for these fields ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_property_id","fields":{"NAME":"New Property Name"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.property.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_property_id","fields":{"NAME":"New Property Name"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.product.property.update
    ```

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const propertyName = prompt("Enter new name");
    
    	const response = await $b24.callMethod(
    		"crm.product.property.update",
    		{
    			id: id,
    			fields:
    			{
    				"NAME": propertyName
    			}
    		}
    	);
    
    	const result = response.getData().result;
    	console.dir(result);
    	if(response.more())
    		response.next();
    }
    catch(error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    $id = readline("Enter ID");
    $propertyName = readline("Enter new name");
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.product.property.update',
                [
                    'id' => $id,
                    'fields' => [
                        'NAME' => $propertyName
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
            if ($result->more()) {
                $result->next();
            }
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating product property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    var propertyName = prompt("Enter new name");
    BX24.callMethod(
        "crm.product.property.update",
        {
            id: id,
            fields:
            {
                "NAME": propertyName
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $id = 'your_property_id'; // Replace 'your_property_id' with the actual property ID
    $propertyName = 'New Property Name'; // Replace 'New Property Name' with the new name

    $result = CRest::call(
        'crm.product.property.update',
        [
            'id' => $id,
            'fields' => [
                'NAME' => $propertyName
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}
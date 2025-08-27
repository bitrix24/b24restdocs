# Get a set of contacts associated with the specified company crm.company.contact.items.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameters or fields are missing
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is missing
- error response is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the method: any user

The method `crm.company.contact.items.get` returns a set of contacts associated with the specified company.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Company identifier. ||
|#

The result is returned as an array of objects, each containing the following fields:

#|
|| **Field** | **Description** ||
|| **CONTACT_ID**
[`unknown`](../../../data-types.md) | Contact identifier ||
|| **SORT**
[`unknown`](../../../data-types.md) | Sort index ||
|| **ROLE_ID**
[`unknown`](../../../data-types.md) | Role identifier (reserved) ||
|| **IS_PRIMARY**
[`unknown`](../../../data-types.md) | Primary contact flag ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.company.contact.items.get",
    		{
    			id: id
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
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
                'crm.company.contact.items.get',
                [
                    'id' => $id
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
        echo 'Error getting company contact items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.company.contact.items.get",
        {
            id: id
        },
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}
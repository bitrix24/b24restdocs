# Update Custom Field for Quotes crm.quote.userfield.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- corrections needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.userfield.update` updates an existing custom field for quotes.

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the custom field. ||
|| **fields**
[`unknown`](../../../data-types.md) | Set of fields - an array of the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values returned by the method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md).
||
|#

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const label = prompt("Enter new name");
    
    	const response = await $b24.callMethod(
    		"crm.quote.userfield.update",
    		{
    			id: id,
    			fields:
    			{
    				"EDIT_FORM_LABEL": label,
    				"LIST_COLUMN_LABEL": label
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
    $label = readline("Enter new name");
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.userfield.update',
                [
                    'id' => $id,
                    'fields' => [
                        'EDIT_FORM_LABEL'   => $label,
                        'LIST_COLUMN_LABEL' => $label
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
        if ($response->getResponseData()->more()) {
            $response->getResponseData()->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    var label = prompt("Enter new name");
    BX24.callMethod(
        "crm.quote.userfield.update",
        {
            id: id,
            fields:
            {
                "EDIT_FORM_LABEL": label,
                "LIST_COLUMN_LABEL": label
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

{% endlist %}

{% include [Examples Note](../../../../_includes/examples.md) %}
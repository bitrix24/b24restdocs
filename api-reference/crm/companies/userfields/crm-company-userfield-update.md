# Update Existing User Field for Companies crm.company.userfield.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing
- links to pages that have not yet been created are not specified (crm.userfield.fields)

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.userfield.update` updates an existing user field for companies.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the user field. ||
|| **fields**
[`unknown`](../../../data-types.md) | Set of fields - an array in the form array("field to update"=>"value"[, ...]), where "field to update" can take values returned by the method `crm.userfield.fields`. ||
|| **LIST**
[`unknown`](../../../data-types.md) | Contains a set of list values for user fields of type List. Specified when creating/updating the field. Each value is an array with the following fields:
- **VALUE** - value of the list item. This field is required when creating a new item.
- **SORT** - sorting.
- **DEF** - if equal to `Y`, the list item is the default value. For multiple fields, several `DEF=Y` are allowed. For non-multiple fields, the first will be considered default.
- **XML_ID** - external code of the value. This parameter is considered only when updating already existing values of the list item.
- **ID** - identifier of the value. If specified, it is considered that this is an update of an existing list item value, not the creation of a new one. Makes sense only when calling `*.userfield.update` methods.
- **DEL** - if equal to `Y`, the existing list item will be deleted. Applied if the `ID` parameter is filled. ||
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
    		"crm.company.userfield.update",
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
    $id = $_POST['id'];
    $label = $_POST['label'];
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.userfield.update',
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
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Data: ' . print_r($result->data(), true);
            if ($result->more()) {
                $result->next();
            }
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
        "crm.company.userfield.update",
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}
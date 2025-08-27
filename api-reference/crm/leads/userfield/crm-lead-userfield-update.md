# Update Field crm.lead.userfield.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- edits needed to meet writing standards
- parameter requirements are not indicated
- examples are missing (in other languages)
- success response is missing
- error response is missing
- link to the yet-to-be-created page is not specified

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.userfield.update` updates an existing custom field for leads.

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the custom field. ||
|| **fields** | Set of fields - an array in the form `array("field"=>"value"[, ...])`, where "updated field" can take values returned by the method [crm.userfield.fields](.). ||
|| **LIST** | Contains a set of list values for custom fields of type List. Specified when creating/updating the field. Each value is an array with the following fields: 
- **VALUE** - value of the list item. This field is required when creating a new item. 
- **SORT** - sorting. 
- **DEF** - if equal to `Y`, the list item is the default value. For multiple fields, several `DEF=Y` are allowed. For non-multiple fields, the first will be considered default. 
- **XML_ID** - external code of the value. This parameter is considered only when updating already existing values of the list item. 
- **ID** - identifier of the value. If specified, it is considered that this is an update of an existing value of the list item, not the creation of a new one. 
- **DEL** - if equal to `Y`, the existing list item will be deleted. Used if the ID parameter is filled. ||
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
    		"crm.lead.userfield.update",
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
                'crm.lead.userfield.update',
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
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            print_r($result->data());
            if ($result->more()) {
                $result->next();
            }
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating lead user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    var label = prompt("Enter new name");
    BX24.callMethod(
        "crm.lead.userfield.update",
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
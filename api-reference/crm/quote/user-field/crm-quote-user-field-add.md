# Create a custom field for estimates crm.quote.userfield.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.userfield.add` creates a new custom field for estimates.

The system limit for the field name is 20 characters. The custom field name always has the prefix `UF_CRM_`, meaning the actual length of the name is 13 characters.

#|
||  **Parameter** / **Type**| **Description** ||
|| **fields**
[`unknown`](../../../data-types.md) | A set of fields - an array of the form `array("field"=>"value"[, ...])`, containing the description of the custom field. A complete description of the fields can be obtained by calling the method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md). 
||
|#

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.quote.userfield.add',
    		{
    			fields:
    			{
    				"FIELD_NAME": "MY_STRING",
    				"EDIT_FORM_LABEL": "My String",
    				"LIST_COLUMN_LABEL": "My String",
    				"USER_TYPE_ID": "string",
    				"XML_ID": "MY_STRING",
    				"SETTINGS": { "DEFAULT_VALUE": "Hello, world!" }
    			}
    		}
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
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.userfield.add',
                [
                    'fields' => [
                        'FIELD_NAME'       => 'MY_STRING',
                        'EDIT_FORM_LABEL'  => 'My String',
                        'LIST_COLUMN_LABEL' => 'My String',
                        'USER_TYPE_ID'     => 'string',
                        'XML_ID'           => 'MY_STRING',
                        'SETTINGS'         => ['DEFAULT_VALUE' => 'Hello, world!'],
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
        echo 'Error adding user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.quote.userfield.add",
        {
            fields:
            {
                "FIELD_NAME": "MY_STRING",
                "EDIT_FORM_LABEL": "My String",
                "LIST_COLUMN_LABEL": "My String",
                "USER_TYPE_ID": "string",
                "XML_ID": "MY_STRING",
                "SETTINGS": { "DEFAULT_VALUE": "Hello, world!" }
            }
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

{% include [Footnote about examples](../../../../_includes/examples.md) %}
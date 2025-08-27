# Create a new custom field for companies crm.company.userfield.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- corrections needed for standard writing
- parameter types are not specified
- parameter mandatory status is not indicated
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created (crm.userfield.fields) are not specified

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the method: any user

The method `crm.company.userfield.add` creates a new custom field for companies.

The system limitation on the field name is 20 characters. The custom field name always has the prefix UF_CRM_, meaning the actual length of the name is 13 characters.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`array`](../../../data-types.md) | A set of fields - an array of the form array("field"=>"value"[, ...]), containing the description of the custom field. ||
|| **LIST**
[`unknown`](../../../data-types.md) | Contains a set of list values for custom fields of type List. It is specified when creating/updating the field. Each value is an array with the fields: 
- **VALUE** - the value of the list item. This field is mandatory when creating a new item. 
- **SORT** - sorting.
- **DEF** - if equal to Y, the list item is the default value. For multiple fields, several DEF=Y are allowed. For non-multiple fields, the first will be considered default.
- **XML_ID** - external code of the value. This parameter is considered only when updating already existing values of the list item.
- **ID** - identifier of the value. If specified, it is considered an update of an existing list item value, not the creation of a new one. It only makes sense when calling the `*.userfield.update` methods.
- **DEL** - if equal to Y, the existing list item will be deleted. This is applied if the ID parameter is filled. ||
|#

A complete description of the fields can be obtained by calling the method `crm.userfield.fields`.

## Examples

**Example #1: Creating a text field**

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.company.userfield.add",
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
    	console.dir(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.userfield.add',
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
        echo 'Error adding company user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.userfield.add",
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


**Example #2: Creating a list**

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.company.userfield.add",
    		{
    			fields:
    			{
    				"FIELD_NAME": "MY_LIST",
    				"EDIT_FORM_LABEL": "My List",
    				"LIST_COLUMN_LABEL": "My List",
    				"USER_TYPE_ID": "enumeration",
    				"LIST": [
    					{ "VALUE": "Item #1" },
    					{ "VALUE": "Item #2" },
    					{ "VALUE": "Item #3" },
    					{ "VALUE": "Item #4" },
    					{ "VALUE": "Item #5" }
    				],
    				"XML_ID": "MY_LIST",
    				"SETTINGS": { "LIST_HEIGHT": 3 }
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.userfield.add',
                [
                    'fields' => [
                        'FIELD_NAME'       => 'MY_LIST',
                        'EDIT_FORM_LABEL'  => 'My List',
                        'LIST_COLUMN_LABEL' => 'My List',
                        'USER_TYPE_ID'     => 'enumeration',
                        'LIST'             => [
                            ['VALUE' => 'Item #1'],
                            ['VALUE' => 'Item #2'],
                            ['VALUE' => 'Item #3'],
                            ['VALUE' => 'Item #4'],
                            ['VALUE' => 'Item #5'],
                        ],
                        'XML_ID'           => 'MY_LIST',
                        'SETTINGS'         => ['LIST_HEIGHT' => 3],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding company user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.userfield.add",
        {
            fields:
            {
                "FIELD_NAME": "MY_LIST",
                "EDIT_FORM_LABEL": "My List",
                "LIST_COLUMN_LABEL": "My List",
                "USER_TYPE_ID": "enumeration",
                "LIST": [
                    { "VALUE": "Item #1" },
                    { "VALUE": "Item #2" },
                    { "VALUE": "Item #3" },
                    { "VALUE": "Item #4" },
                    { "VALUE": "Item #5" }
                    ],
                "XML_ID": "MY_LIST",
                "SETTINGS": { "LIST_HEIGHT": 3 }
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
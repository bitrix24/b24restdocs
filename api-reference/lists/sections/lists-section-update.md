# Update the universal list section lists.section.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits are needed to meet writing standards
- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.section.update` updates a list section. On successful update, the response is `true`, otherwise *Exception*.

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | Identifier of the information block type (required). Possible values: 
- lists - list information block type 
- bitrix_processes - processes information block type 
- lists_socnet - group lists information block type | ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | Code or identifier of the information block (required). | ||
|| **IBLOCK_SECTION_ID**
[`unknown`](../../data-types.md) | Identifier of the parent section; if not specified, the section will be root | ||
|| **FIELDS** | Array of fields and values. Required fields: NAME | ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); | ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.section.update',
    		{
    			'IBLOCK_TYPE_ID': 'lists',
    			'IBLOCK_CODE': 'rest_1',
    			'SECTION_CODE': 'Section_code_1',
    			'FIELDS': {
    				'NAME': 'Section_1 (Updated)'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	alert("Error: " + error);
    }
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'lists.section.update',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_CODE'    => 'rest_1',
                    'SECTION_CODE'   => 'Section_code_1',
                    'FIELDS'         => [
                        'NAME' => 'Section_1 (Updated)'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    /* lists.section.update */
    var params = {
        'IBLOCK_TYPE_ID': 'lists',
        'IBLOCK_CODE': 'rest_1',
        'SECTION_CODE': 'Section_code_1',
        'FIELDS': {
            'NAME': 'Section_1 (Updated)'
        }
    };
    BX24.callMethod(
        'lists.section.update',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                console.log(result.data());
        }
    );
    ```

{% endlist %}

{% include [Example notes](../../../_includes/examples.md) %}
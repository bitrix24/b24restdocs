# Delete the section of the universal list lists.section.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.section.delete` removes a list section. On successful deletion, the response is `true`, otherwise *Exception*.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | Identifier of the information block type (required). Possible values: 
- lists - list information block type 
- bitrix_processes - processes information block type 
- lists_socnet - group lists information block type | ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | Code or identifier of the information block (required). | ||
|| **SECTION_CODE/SECTION_ID**^*^
[`unknown`](../../data-types.md) | Code or identifier of the section (required). | ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); | ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.section.delete',
    		{
    			'IBLOCK_TYPE_ID': 'lists',
    			'IBLOCK_CODE': 'rest_1',
    			'SECTION_CODE': 'Section_code_1'
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
        $params = [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_CODE'    => 'rest_1',
            'SECTION_CODE'   => 'Section_code_1',
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'lists.section.delete',
                $params
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
        echo 'Error deleting section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    /* lists.section.delete */
    var params = {
        'IBLOCK_TYPE_ID': 'lists',
        'IBLOCK_CODE': 'rest_1',
        'SECTION_CODE': 'Section_code_1'
    };
    BX24.callMethod(
        'lists.section.delete',
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

{% include [Footnote about examples](../../../_includes/examples.md) %}
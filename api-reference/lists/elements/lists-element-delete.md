# Delete List Element lists.element.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.element.delete` removes a list element. Upon successful deletion, the response is `true`, otherwise *Exception*.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the information block (required):
- **lists** - list information block type
- **bitrix_processes** - processes information block type
- **lists_socnet** - group lists information block type ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | code or `id` of the information block (required) ||
|| **ELEMENT_CODE/ELEMENT_ID**
[`unknown`](../../data-types.md) | code or `id` of the element ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.element.delete',
    		{
    			'IBLOCK_TYPE_ID': 'lists_socnet',
    			'IBLOCK_CODE': 'rest_1',
    			'ELEMENT_CODE': 'element_1'
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		alert("Error: " + result.error());
    	else
    		alert("Success: " + result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP


    ```php
    try {
        $params = [
            'IBLOCK_TYPE_ID' => 'lists_socnet',
            'IBLOCK_CODE'    => 'rest_1',
            'ELEMENT_CODE'   => 'element_1',
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'lists.element.delete',
                $params
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists_socnet',
        'IBLOCK_CODE': 'rest_1',
        'ELEMENT_CODE': 'element_1'
    };
    BX24.callMethod(
        'lists.element.delete',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

{% endlist %}

{% include [Example Note](../../../_includes/examples.md) %}
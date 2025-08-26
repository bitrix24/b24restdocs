# Get Available Field Types for lists.field.type.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

The method `lists.field.type.get` allows you to retrieve a list of available field types for the specified list. On success, a list of available field types will be returned; otherwise, an empty array.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the information block type (required):
- **lists** - list information block type
- **bitrix_processes** - processes information block type
- **lists_socnet** - group lists information block type ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | code or `id` of the information block (required) ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group) ||
|| **FIELD_ID**
[`unknown`](../../data-types.md) | `ID` of the field (If the field is a property of the information block, the format is: `PROPERTY_propertyId`. If specified, returns available types for the specified field type) ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.field.type.get',
    		params
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		alert("getFieldTypes: " + result.error());
    	}
    	else
    	{
    		var types = result.data(), html = '';
    		for(var typeId in types)
    		{
    			if(!types.hasOwnProperty(typeId)) continue;
    			html += ''+types[typeId]+''; 
    		}
    		$('#field-type').html(html);
    	}
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
        ];
    
        if (!empty($params['IBLOCK_CODE']) || !empty($params['IBLOCK_ID'])) {
            $response = $b24Service
                ->core
                ->call(
                    'lists.field.type.get',
                    $params
                );
    
            $result = $response
                ->getResponseData()
                ->getResult();
    
            $html = '';
            foreach ($result as $typeId => $type) {
                $html .= '' . $type . '';
            }
    
            echo 'Success: ' . $html;
            $('#field-type').html($html);
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting field types: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists_socnet',
        'IBLOCK_CODE': 'rest_1'
    };
    if(!params.IBLOCK_CODE && !params.IBLOCK_ID)
        return;
    BX24.callMethod(
        'lists.field.type.get',
        params,
        function(result)
        {
            if(result.error())
            {
                alert("getFieldTypes: " + result.error());
            }
            else
            {
                var types = result.data(), html = '';
                for(var typeId in types)
                {
                    if(!types.hasOwnProperty(typeId)) continue;
                        html += ''+types[typeId]+''; 
                }
                $('#field-type').html(html);
            }
        }
    );
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}
# Get Data About List Fields lists.field.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.field.get` allows you to retrieve data about a field or fields of a list. On success, a list of fields with data will be returned; otherwise, an empty array will be returned.

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
[`unknown`](../../data-types.md) | `ID` of the field (If the field is a property of the information block, the format is: "PROPERTY_propertyId". If not specified, the response will include data for all fields of the specified list) ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.field.get',
    		{
    			'IBLOCK_TYPE_ID': 'lists_socnet',
    			'IBLOCK_CODE': 'rest_1'
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
            'IBLOCK_TYPE_ID' => 'lists_socnet',
            'IBLOCK_CODE'    => 'rest_1',
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'lists.field.get',
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
        echo 'Error calling lists.field.get: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists_socnet',
        'IBLOCK_CODE': 'rest_1'
    };
    BX24.callMethod(
        'lists.field.get',
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
# Get parameters of the section or list of sections `lists.section.get`

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- corrections needed for writing standards
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.section.get` returns a list of sections or a section. On success, it returns data for the section(s), otherwise an empty array.

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | Identifier of the information block type (required). Possible values: 
- lists - type of list information block 
- bitrix_processes - type of processes information block 
- lists_socnet - type of group lists information block | ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | Code or identifier of the information block (required). | ||
|| **FILTER**
[`unknown`](../../data-types.md) | Array of fields and values for filtering. Fields from the filter `CIBlockSection::GetList` are available for filtering. | ||
|| **SELECT**
[`unknown`](../../data-types.md) | Array of fields to select. Available fields are described in the documentation `CIBlockSection::GetList` | ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | Group identifier (required if the list is created for a group) | ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

(**) - except for user-defined (UF_) fields.

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.section.get',
    		{
    			'IBLOCK_TYPE_ID': 'lists',
    			'IBLOCK_CODE': 'rest_1',
    			'FILTER': {
    				'NAME': 'section_%'
    			},
    			'SELECT': ['ID', 'NAME']
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
            'FILTER'         => [
                'NAME' => 'section_%'
            ],
            'SELECT'         => ['ID', 'NAME']
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'lists.section.get',
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
        echo 'Error getting section list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    /* lists.section.get */
    var params = {
        'IBLOCK_TYPE_ID': 'lists',
        'IBLOCK_CODE': 'rest_1',
        'FILTER': {
            'NAME': 'section_%'
        },
        'SELECT': ['ID', 'NAME']
    };
    BX24.callMethod(
        'lists.section.get',
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
# Get Infoblock Data lists.get

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

The `lists.get` method returns infoblock data. On success, infoblock data will be returned; otherwise, an empty array will be returned. This method allows you to retrieve a list of data from all infoblocks of the specified type. When working with social network groups, it is essential to specify the `ID` of the group; otherwise, an access error will occur.

## Parameters
#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the infoblock type (required):
- **lists** - list infoblock type
- **bitrix_processes** - processes infoblock type
- **lists_socnet** - group lists infoblock type ||
|| **IBLOCK_CODE/IBLOCK_ID**
[`unknown`](../../data-types.md) | code or `id` of the infoblock (if not specified, data for all lists of the specified infoblock type will be returned) ||
|| **SOCNET_GROUP_ID**
[`unknown`](../../data-types.md) | `id` of the group, required if the list is in groups. ||
|| **IBLOCK_ORDER**
[`unknown`](../../data-types.md) | Sorting. Array of fields for sorting the infoblock sections. Sorting direction: **asc** (ascending) or **desc** (descending) Example: 
`'IBLOCK_ORDER': { "ID": "DESC" }` ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.get',
    		{
    			'IBLOCK_TYPE_ID': 'lists',
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
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_CODE'    => 'rest_1',
        ];
    
        $response = $b24Service
            ->core
            ->call('lists.get', $params);
    
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
        echo 'Error calling lists.get: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists',
        'IBLOCK_CODE': 'rest_1'
    };
    BX24.callMethod(
        'lists.get',
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
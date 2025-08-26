# Get the ID of the information block type lists.get.iblock.type.id

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.get.iblock.type.id` returns the `id` of the information block type. On success, a string with the `id` of the information block type will be returned; otherwise, `null`.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **IBLOCK_CODE/IBLOCK_ID**
[`unknown`](../../data-types.md) | code or `id` of the information block | ||
|#

## Example:

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.get.iblock.type.id',
    		{
    			'IBLOCK_ID': '41'
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
            'IBLOCK_ID' => '41'
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'lists.get.iblock.type.id',
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
        echo 'Error calling lists.get.iblock.type.id: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        'IBLOCK_ID': '41'
    };
    BX24.callMethod(
        'lists.get.iblock.type.id',
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

{% include [Examples note](../../../_includes/examples.md) %}
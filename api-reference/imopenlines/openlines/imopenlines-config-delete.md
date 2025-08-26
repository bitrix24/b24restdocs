# Delete open line imopenlines.config.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method deletes an open line.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CONFIG_ID***
[`unknown`](../../data-types.md) | Line identifier ||
|#

## Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS


    ```js
    try
    {
    	const params = {
    		CONFIG_ID: 1
    	};
    	
    	const response = await $b24.callMethod(
    		'imopenlines.config.delete',
    		params
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		alert("Error: " + result.error());
    	}
    	else
    	{
    		alert("Success: " + result);
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
            'CONFIG_ID' => 1
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.config.delete',
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
        echo 'Error deleting configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //imopenlines.config.delete
    function configDelete()
    {
        var params = {
            CONFIG_ID: 1
        };
        BX24.callMethod(
            'imopenlines.config.delete',
            params,
            function (result) {
                if (result.error())
                    alert("Error: " + result.error());
                else
                    alert("Success: " + result.data());
            }
        );
    }
    ```

- PHP CRest

    // example for php

{% endlist %}
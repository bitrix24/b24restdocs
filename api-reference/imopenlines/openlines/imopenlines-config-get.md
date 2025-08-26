# Get open line by Id imopenlines.config.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method retrieves an open line by its ID.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CONFIG_ID***  
[`unknown`](../../data-types.md) | Line ID ||
|| **WITH_QUEUE**  
[`unknown`](../../data-types.md) | Display with the list of line operators (Y/N, default: Y) ||
|| **SHOW_OFFLINE**  
[`unknown`](../../data-types.md) | Show the list along with operators who are offline (Y/N, default: Y) ||
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
    		CONFIG_ID:    1,
    		WITH_QUEUE: 'Y',
    		SHOW_OFFLINE: 'Y'
    	};
    	
    	const response = await $b24.callMethod(
    		'imopenlines.config.get',
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
            'CONFIG_ID'   => 1,
            'WITH_QUEUE'  => 'Y',
            'SHOW_OFFLINE' => 'Y',
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.config.get',
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
        echo 'Error calling imopenlines.config.get: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //imopenlines.config.get
    function configGet()
    {
        var params = {
            CONFIG_ID:    1,
            WITH_QUEUE: 'Y',
            SHOW_OFFLINE: 'Y'
        };
        BX24.callMethod(
            'imopenlines.config.get',
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
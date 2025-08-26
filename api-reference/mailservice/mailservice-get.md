# Get parameters of the mail service mailservice.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing descriptions of parameter types
- no response examples

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.get` returns the parameters of the specified mail service.

## Parameters

#|
||  **Parameter** / **Type**| **Description** | **Available since** ||
|| **ID**
[`unknown`](../data-types.md) | Identifier of the mail service | ||
|#

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"mailservice.get",
    		{
    			'ID': 10
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch(error)
    {
    	console.error(error);
    }
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'mailservice.get',
                [
                    'ID' => 10
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting mail service: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "mailservice.get",
        {
            'ID': 10
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}
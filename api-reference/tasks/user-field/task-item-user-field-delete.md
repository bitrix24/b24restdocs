# Delete User Field task.item.userfield.delete

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- missing 1 example (should be three examples - curl, js, php)
- no response in case of error
- no response in case of success

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `task.item.userfield.delete` removes a property.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|| **ID**
[`unknown`](../../data-types.md) | Identifier of the user field. ||
|#

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'task.item.userfield.delete',
    		{
    			'auth': 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
    			'ID': 77
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    	console.log(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.item.userfield.delete',
                [
                    'auth' => 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
                    'ID'   => 77
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
        echo 'Error deleting user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.item.userfield.delete',
        {
            'auth': 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
            'ID': 77
        },
    
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- cURL

    ```http
    $appParams = array(
        'auth' => 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
        'ID' => 77
    );
    ```

    ```http
    $request = 'http://your-domain.com/rest/task.item.userfield.delete.xml?' . http_build_query($appParams);
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}
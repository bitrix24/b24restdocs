# Get a list of available data types task.item.userfield.gettypes

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- one example is missing (there should be three examples - curl, js, php)
- no success response is provided
- no error response is provided
- add a note that other types cannot be added to tasks

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.gettypes` returns all available data types in the system.

Custom fields in tasks support the following data types:
- `string` — string
- `double` — number
- `date` — date
- `boolean` — yes/no 

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'task.item.userfield.gettypes',
    		{'auth': 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa'}
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
                'task.item.userfield.gettypes',
                [
                    'auth' => 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
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
        echo 'Error getting user field types: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.item.userfield.gettypes',
        {'auth': 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa'},

        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- cURL

    ```http
    $request = 'http://your-domain.com/rest/task.item.userfield.gettypes.xml?' . http_build_query($appParams);
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}
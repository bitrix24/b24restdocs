# Update User Field task.item.userfield.update

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- one example is missing (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `task.item.userfield.update` is used to edit the properties' parameters.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|| **ID**
[`unknown`](../../data-types.md) | Identifier of the user field. ||
|| **DATA**
[`unknown`](../../data-types.md) | Array `array('field'=>'value', ...)`. Contains values of the parameters to be edited. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'task.item.userfield.update',
    		{
    			'auth': 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
    			'ID': 77,
    			'DATA': {'XML_ID': 'new_external_id'}
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
                'task.item.userfield.update',
                [
                    'auth' => 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
                    'ID' => 77,
                    'DATA' => ['XML_ID' => 'new_external_id']
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
        echo 'Error updating user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.item.userfield.update',
        {
            'auth': 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
            'ID': 77,
            'DATA': {'XML_ID': 'new_external_id'}
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
        'ID' => 77,
        'DATA' => array('XML_ID' => 'new_external_id')
    );
    ```

    ```http
    $request = 'http://your-domain.com/rest/task.item.userfield.update.xml?' . http_build_query($appParams);
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}
# Get storage parameters or list of all storages entity.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.get` method retrieves the storage parameters or a list of all application storages.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**
[`string`](../../data-types.md) | String identifier of the required storage. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod('entity.get');
    	const result = response.getData().result;
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
                'entity.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling entity.get: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod('entity.get');
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.get.json?auth=59efe32d01c0e9dc5732e8dfa68a4baa
    ```

{% endlist %}

Example of correctly retrieving a list of all available storages:

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.get',
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.info('List of created storages:', result);
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
                'entity.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'List of created storages: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling entity.get: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "entity.get",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info("List of created storages:", result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}

## Response in case of success

> 200 OK
```json
{"result":
    [
        {
            "ENTITY":"dish",
            "NAME":"Dishes"
        },
        {
            "ENTITY":"menu",
            "NAME":"Menu"
        }
    ]
}
```
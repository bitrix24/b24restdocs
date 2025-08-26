# Change Group Owner sonet_group.setowner

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of error
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method changes the owner of a group. It can be executed either by the social network administrator or the current group owner.

## Parameters:

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | The identifier of the group whose owner is being changed. ||
|| **USER_ID** | The identifier of the new owner. ||
|#

{% include [Note on parameters](../../_includes/required.md) %}

On success, it returns `true`.

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sonet_group.setowner',
    		{
    			'GROUP_ID': 11,
    			'USER_ID': 2
    		}
    	);
    	
    	const result = response.getData().result;
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
                'sonet_group.setowner',
                [
                    'GROUP_ID' => 11,
                    'USER_ID'  => 2
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting group owner: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sonet_group.setowner', {
        'GROUP_ID': 11,
        'USER_ID': 2
    });
    ```

{% endlist %}

{% include [Note on examples](../../_includes/examples.md) %}
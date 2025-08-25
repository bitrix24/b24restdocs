# Unmute the "Silent" Mode tasks.task.unmute

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.unmute` disables the "Silent" mode for a task.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id^*^**
[`unknown`](../data-types.md) | Task identifier. ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Return Value

Returns a JSON array of data about the task (similar to the method [`tasks.task.get`](./tasks-task-get.md)).

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.task.unmute',
    		{
    			id: 1223
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Result:', result);
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
                'tasks.task.unmute',
                [
                    'id' => 1223,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error unmuting task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('tasks.task.unmute', {id: 1223})
    ```

{% endlist %}

{% include [Example Notes](../../_includes/examples.md) %}
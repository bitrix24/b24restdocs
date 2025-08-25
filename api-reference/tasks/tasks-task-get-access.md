# Check Access to Task tasks.task.getaccess

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
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

The method `tasks.task.getaccess` is used to check access to a task.

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|| **users**
[`unknown`](../data-types.md) | Array of user IDs for whom access needs to be checked. By default, the current user is used. ||

|#

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.task.getaccess',
    		{taskId: 1, users: [1]}
    	);
    	
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
                'tasks.task.getaccess',
                [
                    'taskId' => 1,
                    'users'  => [1],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo $result['answer']['result'];
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting task access: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.getaccess',
        {taskId:1, users:[1]},
        function(res){console.log(res.answer.result);}
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../_includes/examples.md) %}
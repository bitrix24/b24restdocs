# Get a List of Methods task.item.* with Their Descriptions task.item.getmanifest

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns a list of methods in the format `task.item.*` along with their descriptions.

The return value of this method is not intended for automated processing, as its format may change without notice.

This method can be useful as a reference, as it always contains up-to-date information.

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [tasks.task.getFields](../../tasks-task-get-fields.md).

{% endnote %}

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.getmanifest
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.getmanifest
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'task.item.getmanifest',
    		[]
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
                'task.item.getmanifest',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic here
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting task manifest: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.item.getmanifest',
        [],
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.item.getmanifest',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}
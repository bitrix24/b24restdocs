# Add a comment to the task task.comment.add

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds comments to a task.

{% note warning %}

Instead of this method, you should use the methods [`task.commentitem.*`](../comment-item/index.md).

{% endnote %}

## Method parameters

#|
|| **Name** | **Description** ||
|| **TASKID** | Task identifier ||
|| **COMMENTTEXT** | Comment ||
|#

It is mandatory to follow the order of parameters in the request. If this order is violated, the request will be executed with errors.

## Code examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":1,"FIELDS":{"POST_MESSAGE":"comment text"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.comment.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":1,"FIELDS":{"POST_MESSAGE":"comment text"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.comment.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'task.comment.add',
    		[1, 'comment text']
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
                'task.comment.add',
                [
                    1,
                    'comment text',
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
        echo 'Error adding task comment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.comment.add',
        [1, 'comment text'],
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
        'task.comment.add',
        [
            'TASKID' => 1,
            'FIELDS' => [
                'POST_MESSAGE' => 'comment text'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}
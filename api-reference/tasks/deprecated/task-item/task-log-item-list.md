# View Task Change History task.logitem.list

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the change history of a task.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name** | **Description** ||
|| **ID*** | Task identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":1205}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.logitem.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":1205,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.logitem.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'task.logitem.list',
        [1205],
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('task.logitem.list', [1205], 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('task.logitem.list', [1205], 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.logitem.list',
                [1205]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching task log items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.logitem.list',
        [1205],
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.logitem.list',
        ['taskId' => 1205]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
Array
(
    [result] => Array
        (
            [0] => Array
                (
                    [CREATED_DATE] => 2022-12-09T11:10:49+02:00
                    [USER_ID] => 1
                    [TASK_ID] => 1223
                    [FIELD] => NEW
                    [FROM_VALUE] =>
                    [TO_VALUE] =>
                )

            [1] => Array
                (
                    [CREATED_DATE] => 2022-12-09T11:11:29+02:00
                    [USER_ID] => 1
                    [TASK_ID] => 1223
                    [FIELD] => COMMENT
                    [FROM_VALUE] =>
                    [TO_VALUE] => 1247
                )

            [2] => Array
                (
                    [CREATED_DATE] => 2022-12-09T11:11:40+02:00
                    [USER_ID] => 1
                    [TASK_ID] => 1223
                    [FIELD] => TAGS
                    [FROM_VALUE] =>
                    [TO_VALUE] => this is confidential
                )
        )

    [time] => Array
        (
            [start] => 1670577445.3012
            [finish] => 1670577447.9552
            [duration] => 2.6539649963379
            [processing] => 2.2574801445007
            [date_start] => 2022-12-09T11:17:25+02:00
            [date_finish] => 2022-12-09T11:17:27+02:00
            [operating] => 2.2573609352112
        )
)
```


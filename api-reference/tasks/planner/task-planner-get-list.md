# Get the list of tasks from the Planner for the day task.planner.getlist

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.planner.getlist` retrieves a list of task identifiers from the "Planner for the day" of the current user. To get detailed information about the tasks, use the method [tasks.task.get](../tasks-task-get.md).

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.planner.getlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.planner.getlist
    ```

- JS

    ```javascript
    // callListMethod is recommended when you need to retrieve
    // the entire set of list data and the volume of records is relatively small
    // (up to about 1000 items). The method loads all data at once, which
    // can lead to high memory load when working with large volumes.

    try {
        const response = await $b24.callListMethod(
        "task.planner.getlist",
        {},
        (progress) => {
            console.log("Progress:", progress);
        }
        );
        const items = response.getData() || [];
        for (const entity of items) {
        console.log("Entity:", entity);
        }
    } catch (error) {
        console.error("Request failed", error);
    }

    // fetchListMethod is preferable when working with large datasets.
    // The method implements iterative selection using a generator, which
    // allows processing data in parts and efficiently using memory.

    try {
        const generator = $b24.fetchListMethod("task.planner.getlist", {}, "ID");
        for await (const page of generator) {
        for (const entity of page) {
            console.log("Entity:", entity);
        }
        }
    } catch (error) {
        console.error("Request failed", error);
    }

    // callMethod provides manual control over the pagination
    // data retrieval process through the start parameter. It is suitable for scenarios where
    // precise control over request batches is required. However, with large
    // volumes of data, it may be less efficient compared to
    // fetchListMethod.

    try {
        const response = await $b24.callMethod("task.planner.getlist", {}, 0);
        const result = response.getData().result || [];
        for (const entity of result) {
        console.log("Entity:", entity);
        }
    } catch (error) {
        console.error("Request failed", error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.planner.getlist',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching task planner list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'task.planner.getlist',
        {},
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, which allows processing data in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('task.planner.getlist', {}, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination data retrieval process through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('task.planner.getlist', {}, 0)
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
                'task.planner.getlist',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result->data(), true);
        echo 'Full Result: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting task planner list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "task.planner.getlist",
        [],
        function (result) {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.planner.getlist',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [7811, 8017, 7789, 8015],
    "time": {
        "start": 1755252195.609436,
        "finish": 1755252195.636649,
        "duration": 0.027212858200073242,
        "processing": 0.0030121803283691406,
        "date_start": "2025-08-15T13:03:15+02:00",
        "date_finish": "2025-08-15T13:03:15+02:00",
        "operating_reset_at": 1755252795,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of task identifiers.

If there are no tasks in the planner for the day, it returns an empty array ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
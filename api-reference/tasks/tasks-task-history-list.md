# Task History: tasks.task.history.list

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of error

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.history.list` retrieves the task history.

It returns an array of data (see examples below).

You can filter and sort by all fields (see [tasks.task.list](./tasks-task-list.md)). By default, it returns the entire history without pagination.

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|| **start**
[`unknown`](../data-types.md) | How many initial records to skip in the result. Due to technical limitations, this parameter's value must always be a multiple of 50. For example, with a value of 50, the result will display the 51st record and subsequent ones, while the first 50 records will be skipped.

Setting the value to -1 will disable the count.

Works for HTTPS requests.||
|#

## Example 1

Output the history of a specific task using the `NEW` filter (i.e., when the task was created):
```js
BX24.callMethod('tasks.task.history.list', {taskId: 119, filter:{FIELD:'NEW'}}, (res)=>{console.log(res.answer.result);});
```

## Successful Response

> 200 OK

```json
{
    "result": {
        "list": [
            {
                "id": "1230",
                "createdDate": "03/01/2019 15:29:28",
                "field": "NEW",
                "value": {
                    "from": null,
                    "to": null
                },
                "user": {
                    "id": "1",
                    "name": "Max",
                    "lastName": "Grechushnikov",
                    "secondName": "",
                    "login": "admin"
                }
            }
        ]
    },
    "time": {
        "start": 1552382093.81029,
        "finish": 1552382093.927268,
        "duration": 0.11697793006896973,
        "processing": 0.018744230270385742,
        "date_start": "2019-03-12T11:14:53+02:00",
        "date_finish": "2019-03-12T11:14:53+02:00"
    }
}
```

## Example 2

Output the history of a specific task without using filters:

{% list tabs %}

- JS

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
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('task.planner.getlist', {}, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
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
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting task planner list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.planner.getlist',
        [],
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Examples Note](../../_includes/examples.md) %}
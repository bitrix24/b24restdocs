# Get the list of checklist items task.checklistitem.getlist

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- no success response is provided
- no error response is provided

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.checklistitem.getlist` returns a list of checklist items in a task.

## Parameters

#|
|| **Parameter** / **Type**| **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ORDER**
[`unknown`](../../data-types.md) | Array for sorting the result. The sorting field can take the following values:
- `ID` — checklist item identifier;
- `CREATED_BY` — identifier of the user who created the item;
- `TOGGLED_BY` — identifier of the user who changed the state of the checklist item;
- `TOGGLED_DATE` — time when the state of the checklist item was changed;
- `TITLE` — title of the checklist item;
- `SORT_INDEX` — sorting index of the item;
- `IS_COMPLETE` — item marked as completed.

The sorting direction can take the following values:
- `asc` — ascending;
- `desc` — descending.

Optional. By default, it is sorted in descending order by checklist item identifier. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

{% note info %}

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'task.checklistitem.getlist',
        [13, {'TOGGLED_DATE': 'desc'}],
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('task.checklistitem.getlist', [13, {'TOGGLED_DATE': 'desc'}], 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. Suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('task.checklistitem.getlist', [13, {'TOGGLED_DATE': 'desc'}], 0)
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
                'task.checklistitem.getlist',
                [
                    13,
                    ['TOGGLED_DATE' => 'desc']
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting checklist items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.getlist',
        [13, {'TOGGLED_DATE': 'desc'}],
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}
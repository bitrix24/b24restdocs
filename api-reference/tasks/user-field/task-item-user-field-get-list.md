# Get a list of custom fields task.item.userfield.getlist

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- one example is missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.getlist` returns a list of properties.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|| **ORDER**
[`unknown`](../../data-types.md) | Array for sorting the result. An array in the form `array('sort field'=>'sort direction' [, ...])`. ||
|| **FILTER**
[`unknown`](../../data-types.md) | Array for filtering the result in the form `array('filtered field'=>'filter value' [, ...])`. Required parameter. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'task.item.userfield.getlist',
        {
          order: { "ID": "ASC" },
          filter: { "EDIT_IN_LIST": "Y" }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('task.item.userfield.getlist', { order: { "ID": "ASC" }, filter: { "EDIT_IN_LIST": "Y" } }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('task.item.userfield.getlist', { order: { "ID": "ASC" }, filter: { "EDIT_IN_LIST": "Y" } }, 0)
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
                'task.item.userfield.getlist',
                [
                    'order' => [
                        'ID' => 'ASC'
                    ],
                    'filter' => [
                        'EDIT_IN_LIST' => 'Y'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        // Your logic for processing data
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting user fields list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "task.item.userfield.getlist",
        {
            order:
            {
                "ID": "ASC"
            },
            filter:
            {
                "EDIT_IN_LIST": "Y"
            }
        },
        function(result)
        {
        }
    );
    ```

- cURL

    ```http    
    $appParams = array(
    'auth' => 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
    'ORDER' => array('ID' => 'asc'),
    'FILTER' => array('USER_TYPE_ID' => 'string')
    );
    ```

    ```http    
    $request = 'http://your-domain.com/rest/task.item.userfield.getlist.xml?' . http_build_query($appParams);
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}


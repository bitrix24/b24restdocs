# Get a list of recurring deal template settings crm.deal.recurring.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.recurring.list` returns a list of recurring deal template settings based on the filter.

When querying, use the mask "*" to select all fields (excluding custom and multiple fields).

See the description of [list methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

## Example

{% list tabs %}

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'crm.deal.recurring.list',
        {
          order: { "DEAL_ID": "ASC" },
          filter: { ">COUNTER_REPEAT": 5 },
          select: [ "ID", "DEAL_ID ", "NEXT_EXECUTION", "LAST_EXECUTION", "CATEGORY_ID", "IS_LIMIT" ]
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
      const generator = $b24.fetchListMethod('crm.deal.recurring.list', { order: { "DEAL_ID": "ASC" }, filter: { ">COUNTER_REPEAT": 5 }, select: [ "ID", "DEAL_ID ", "NEXT_EXECUTION", "LAST_EXECUTION", "CATEGORY_ID", "IS_LIMIT" ] }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.deal.recurring.list', { order: { "DEAL_ID": "ASC" }, filter: { ">COUNTER_REPEAT": 5 }, select: [ "ID", "DEAL_ID ", "NEXT_EXECUTION", "LAST_EXECUTION", "CATEGORY_ID", "IS_LIMIT" ] }, 0)
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
                'crm.deal.recurring.list',
                [
                    'order'  => ['DEAL_ID' => 'ASC'],
                    'filter' => ['>COUNTER_REPEAT' => 5],
                    'select' => ['ID', 'DEAL_ID', 'NEXT_EXECUTION', 'LAST_EXECUTION', 'CATEGORY_ID', 'IS_LIMIT'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        if ($result->more()) {
            $result->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching recurring deals: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.deal.recurring.list",
        {
            order: { "DEAL_ID": "ASC" },
            filter: { ">COUNTER_REPEAT": 5 },
            select: [ "ID", "DEAL_ID ", "NEXT_EXECUTION", "LAST_EXECUTION", "CATEGORY_ID", "IS_LIMIT" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}


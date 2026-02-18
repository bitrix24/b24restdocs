# Get a list of custom fields for estimates by filter crm.quote.userfield.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- need to describe parameters here
- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.userfield.list` returns a list of custom fields for estimates by filter.

#|
||  **Parameter** / **Type**| **Description** ||
|| **order**
[`unknown`](../../../data-types.md) | Sorting fields. ||
|| **filter**
[`unknown`](../../../data-types.md) | Filter fields. ||
|#

## Example

{% list tabs %}

- JS


    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    var id = prompt("Enter ID");
    try {
      const response = await $b24.callListMethod(
        'crm.quote.userfield.list',
        {
          order: { "SORT": "ASC" },
          filter: { "MANDATORY": "N" }
        },
        (progress) => {
          console.log('Progress:', progress);
        }
      );
      const items = response.getData() || [];
      for (const entity of items) {
        console.log('Entity:', entity);
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    var id = prompt("Enter ID");
    try {
      const generator = $b24.fetchListMethod('crm.quote.userfield.list', {
        order: { "SORT": "ASC" },
        filter: { "MANDATORY": "N" }
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) {
          console.log('Entity:', entity);
        }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    var id = prompt("Enter ID");
    try {
      const response = await $b24.callMethod('crm.quote.userfield.list', {
        order: { "SORT": "ASC" },
        filter: { "MANDATORY": "N" }
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) {
        console.log('Entity:', entity);
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP


    ```php
    $id = $_POST['id'];
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.userfield.list',
                [
                    'order'  => ['SORT' => 'ASC'],
                    'filter' => ['MANDATORY' => 'N']
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
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.quote.userfield.list",
        {
            order: { "SORT": "ASC" },
            filter: { "MANDATORY": "N" }
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


# Get a list of available storages disk.storage.getlist

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no error response is provided
- no detailed success example is available

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.getlist` returns a list of available storages.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **filter**
[`unknown`](../../data-types.md) | Optional parameter. Supports filtering by fields specified in [disk.storage.getfields](./disk-storage-get-fields.md) as `USE_IN_FILTER: true`. ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

{% note info %}

See also the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'disk.storage.getlist',
        {
          filter: {
            'ENTITY_TYPE': 'group',
            '%NAME': 'Foot'
          }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('disk.storage.getlist', { filter: { 'ENTITY_TYPE': 'group', '%NAME': 'Foot' } }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('disk.storage.getlist', { filter: { 'ENTITY_TYPE': 'group', '%NAME': 'Foot' } }, 0)
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
                'disk.storage.getlist',
                [
                    'filter' => [
                        'ENTITY_TYPE' => 'group',
                        '%NAME'      => 'Foot'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error searching for group storage: ' . $e->getMessage();
    }
    ```

- BX24.js

  ```js
  //search for a group storage with a name containing "Foot"
  BX24.callMethod(
      "disk.storage.getlist",
      {
          filter: {
              'ENTITY_TYPE': 'group',
              '%NAME': 'Foot'
          }
      },
      function (result)
      {
          if (result.error())
              console.error(result.error());
          else
              console.dir(result.data());
      }
  );
  ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response on success

The response is an array of objects, the structure of which is similar to [disk.storage.get](./disk-storage-get.md).
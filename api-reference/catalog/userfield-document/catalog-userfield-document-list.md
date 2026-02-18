# Get a list of values for custom fields of inventory accounting documents catalog.userfield.document.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- add a link to `userfieldconfig.list`(.)
- required parameters are not specified
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.userfield.document.list(select, filter, order, start)
```

The method returns a list of values for custom fields of inventory accounting documents.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **select**
[`array`](../../data-types.md)| An array of fields to display. `documentType` must be specified – [type of inventory accounting documents](../enum/catalog-enum-get-store-document-types.md). | ||
|| **order** 
[`object`](../../data-types.md)| A list to determine the display order, where the key is the field name and the value is `ASC` or `DESC`. | ||
|| **filter** 
[`object`](../../data-types.md)| A list for filtering. `documentType` must be specified – [type of inventory accounting documents](../enum/catalog-enum-get-store-document-types.md). | ||
|| **start** 
[`string`](../../data-types.md)| The page number for output. Works for HTTPS requests. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Example

In the API, field names are used in the format `field[Field ID in the database]` – for example, `field288`. The field ID can be obtained using the method [`userfieldconfig.list`](.).

{% list tabs %}

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'catalog.userfield.document.list',
        {
          select: [
            'documentType',
            'documentId',
            'field287',
            'field288',
            'field289',
          ],
          filter: {
            'documentType': 'S',
            '>=field287': 10,
          },
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
      const generator = $b24.fetchListMethod('catalog.userfield.document.list', {
        select: [
          'documentType',
          'documentId',
          'field287',
          'field288',
          'field289',
        ],
        filter: {
          'documentType': 'S',
          '>=field287': 10,
        },
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('catalog.userfield.document.list', {
        select: [
          'documentType',
          'documentId',
          'field287',
          'field288',
          'field289',
        ],
        filter: {
          'documentType': 'S',
          '>=field287': 10,
        },
      }, 0)
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
                'catalog.userfield.document.list',
                [
                    'select' => [
                        'documentType',
                        'documentId',
                        'field287',
                        'field288',
                        'field289',
                    ],
                    'filter' => [
                        'documentType' => 'S',
                        '>=field287'   => 10,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
            echo 'Error: ' . $result->error()->ex;
        } else {
            echo 'Success: ' . print_r($result->data(), true);
            $result->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling catalog.userfield.document.list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'catalog.userfield.document.list',
    {
        select: [
            'documentType',
            'documentId',
            'field287',
            'field288',
            'field289',
        ],
        filter:{
            'documentType' : 'S',
            '>=field287': 10,
        },
    },
    function(result)
    {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
        result.next();
    }
    );
    ```

- For HTTPS:

    ```
    https://your_account/rest/catalog.userfield.document.list?auth=_authorization_key_&start=50
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}


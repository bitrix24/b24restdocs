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
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
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
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
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
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. Suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
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
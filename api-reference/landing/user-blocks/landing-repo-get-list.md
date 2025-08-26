# Get the list of custom blocks landing.repo.getList

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.repo.getList` retrieves a list of blocks from the current application.

## Parameters

#|
|| **Parameter** | **Description** | **Available from** ||
|| **params**
[`unknown`](../../data-types.md) | Optional array with optional keys:
- select,
- filter,
- order,
- group,

which contain values from the main fields table of the entity. The table is provided below. ||
|#

## Fields

#|
|| **Field** | **Description** ||
|| **ID**
[`unknown`](../../data-types.md) | Record identifier ||
|| **XML_ID**
[`unknown`](../../data-types.md) | Unique record code. ||
|| **APP_CODE**
[`unknown`](../../data-types.md) | Code of the current application. ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Activity status (Y / N). ||
|| **NAME**
[`unknown`](../../data-types.md) | Title. ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | Description. ||
|| **SECTIONS**
[`unknown`](../../data-types.md) | Symbolic codes of categories. ||
|| **PREVIEW**
[`unknown`](../../data-types.md) | Preview image. ||
|| **MANIFEST**
[`unknown`](../../data-types.md) | Manifest. ||
|| **CONTENT**
[`unknown`](../../data-types.md) | Content. ||
|| **CREATED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who created the record. ||
|| **MODIFIED_BY_ID**
[`unknown`](../../data-types.md) | Identifier of the user who modified the record. ||
|| **DATE_CREATE**
[`unknown`](../../data-types.md) | Creation date. ||
|| **DATE_MODIFY**
[`unknown`](../../data-types.md) | Modification date. ||
|#

## Examples

{% list tabs %}

- JS


    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'landing.repo.getList',
        {
          params: {
            select: [
              'ID', 'NAME', 'MANIFEST'
            ],
            filter: {
              '>ID': '1'
            }
          }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('landing.repo.getList', {
        params: {
          select: [
            'ID', 'NAME', 'MANIFEST'
          ],
          filter: {
            '>ID': '1'
          }
        }
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('landing.repo.getList', {
        params: {
          select: [
            'ID', 'NAME', 'MANIFEST'
          ],
          filter: {
            '>ID': '1'
          }
        }
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
                'landing.repo.getList',
                [
                    'params' => [
                        'select' => [
                            'ID', 'NAME', 'MANIFEST'
                        ],
                        'filter' => [
                            '>ID' => '1'
                        ]
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
        echo 'Error getting landing repository list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repo.getList',
        {
            params: {
                select: [
                    'ID', 'NAME', 'MANIFEST'
                ],
                filter: {
                    '>ID': '1'
                }
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}
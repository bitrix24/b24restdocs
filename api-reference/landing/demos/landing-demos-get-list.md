# Get a list of available partner templates landing.demos.getList

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

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

## Description

The method `landing.demos.getList` retrieves a list of available partner templates for the current application.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **params**
[`unknown`](../../data-types.md) | Optional array with optional keys:
- select
- filter
- order
- group
that contain values from the main fields table of the entity. The table is provided below. ||
|#

## Entity fields

#|
|| **Field** | **Description** ||
|| **ID**
[`unknown`](../../data-types.md) | Record identifier. ||
|| **XML_ID**
[`unknown`](../../data-types.md) | Unique record code. ||
|| **APP_CODE**
[`unknown`](../../data-types.md) | Code of the current application. ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | Activity status (Y / N). ||
|| **TITLE**
[`unknown`](../../data-types.md) | Title. ||
|| **DESCRIPTION**
[`unknown`](../../data-types.md) | Description. ||
|| **PREVIEW_URL**
[`unknown`](../../data-types.md) | Preview URL. ||
|| **TYPE**
[`unknown`](../../data-types.md) | Type of the created site (STORE, PAGE). ||
|| **TPL_TYPE**
[`unknown`](../../data-types.md) | Placed in the site/store (S) or page (P) creation wizard. ||
|| **MANIFEST**
[`unknown`](../../data-types.md) | Manifest. ||
|| **SHOW_IN_LIST**
[`unknown`](../../data-types.md) | Whether to show in the list of templates. ||
|| **PREVIEW / PREVIEW2X / PREVIEW3X**
[`unknown`](../../data-types.md) | Various sizes of previews. ||
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
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'landing.demos.getList',
        {
          params: {
            select: [
              'ID', 'TITLE', 'MANIFEST'
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
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('landing.demos.getList', {
        params: {
          select: [
            'ID', 'TITLE', 'MANIFEST'
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
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('landing.demos.getList', {
        params: {
          select: [
            'ID', 'TITLE', 'MANIFEST'
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
                'landing.demos.getList',
                [
                    'params' => [
                        'select' => [
                            'ID', 'TITLE', 'MANIFEST'
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
        echo 'Error getting list of demos: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.demos.getList',
        {
            params: {
                select: [
                    'ID', 'TITLE', 'MANIFEST'
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

{% include [Examples note](../../../_includes/examples.md) %}


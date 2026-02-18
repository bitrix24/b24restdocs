# Get the list of pages landing.landing.getList

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

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.landing.getList` retrieves a list of pages.

{% note warning %}

Pages marked as deleted do not appear in the selections. To explicitly retrieve them, you need to specify the key `DELETED` with the value Y or N when filtering.

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** | **Available from** ||
|| **params**
[`unknown`](../../../data-types.md) | Optional array with optional keys: **select**, **filter**, **order**, **group**, which contain values from the [main fields](../index.md) of the entity.
Additionally, you can pass flags `get_preview = 1` (return page previews), `get_urls = 1` (return public addresses of pages), `check_area` (return flag IS_AREA indicating whether the page is an included area). ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'landing.landing.getList',
        {
          params: {
            select: [
              'ID', 'TITLE'
            ],
            filter: {
              TITLE: '%services%',
              SITE_ID: 205
            },
            order: {
              ID: 'DESC'
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
      const generator = $b24.fetchListMethod('landing.landing.getList', {
        params: {
          select: [
            'ID', 'TITLE'
          ],
          filter: {
            TITLE: '%services%',
            SITE_ID: 205
          },
          order: {
            ID: 'DESC'
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
      const response = await $b24.callMethod('landing.landing.getList', {
        params: {
          select: [
            'ID', 'TITLE'
          ],
          filter: {
            TITLE: '%services%',
            SITE_ID: 205
          },
          order: {
            ID: 'DESC'
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
                'landing.landing.getList',
                [
                    'params' => [
                        'select' => [
                            'ID', 'TITLE'
                        ],
                        'filter' => [
                            'TITLE'   => '%services%',
                            'SITE_ID' => 205
                        ],
                        'order'  => [
                            'ID' => 'DESC'
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting landing list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.getList',
        {
            params: {
                select: [
                    'ID', 'TITLE'
                ],
                filter: {
                    TITLE: '%services%',
                    SITE_ID: 205
                },
                order: {
                    ID: 'DESC'
                }
            }
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}


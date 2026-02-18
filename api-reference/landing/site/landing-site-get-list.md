# Get the list of sites landing.site.getList

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getList` retrieves a list of sites.

{% note warning %}

Please note that pages marked as deleted do not appear in the selections. To explicitly retrieve them, you need to specify the `DELETED` key with the value Y or N when filtering.

{% endnote %}

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **params**
[`unknown`](../../data-types.md) | Optional array with optional keys: **select**, **filter**, **order**, **group**, which contain values from the [main fields](./base-fields.md) of the entity. | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'landing.site.getList',
        {
          params: {
            select: [
              'ID', 'TITLE', 'DOMAIN.DOMAIN'
            ],
            filter: {
              TITLE: '%services%'
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
      const generator = $b24.fetchListMethod('landing.site.getList', {
        params: {
          select: [
            'ID', 'TITLE', 'DOMAIN.DOMAIN'
          ],
          filter: {
            TITLE: '%services%'
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
      const response = await $b24.callMethod('landing.site.getList', {
        params: {
          select: [
            'ID', 'TITLE', 'DOMAIN.DOMAIN'
          ],
          filter: {
            TITLE: '%services%'
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
                'landing.site.getList',
                [
                    'params' => [
                        'select' => [
                            'ID', 'TITLE', 'DOMAIN.DOMAIN'
                        ],
                        'filter' => [
                            'TITLE' => '%services%'
                        ],
                        'order' => [
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
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getList',
        {
            params: {
                select: [
                    'ID', 'TITLE', 'DOMAIN.DOMAIN'
                ],
                filter: {
                    TITLE: '%services%'
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

{% include [Footnote about examples](../../../_includes/examples.md) %}


# Working with Site Types and Scopes

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet writing standards

{% endnote %}

{% endif %}

## Site Types

Sites can be of the following types.

- Main:
  - PAGE (from Home Page) - regular sites.
  - STORE - stores.
  - SMN - sites used in the Sites24 section in the administrative area of the BUS.

- Additional:
  - KNOWLEDGE – knowledge bases.
  - GROUP – knowledge bases of social network groups.

Currently, the extension of types is not supported.

## Scopes

In addition to the separation function at the component level, there is also a division by rights, known as scopes.

If you are working with **main types**, no action is needed.
If working with **additional types**, you need to set the scope before starting. In the case of REST, this can be done by passing an additional parameter **scope**.

## Example

The example provides a method for retrieving a list of pages, but the rule applies to any other method, including working with rights and entity modifications.

{% list tabs %}

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    const params = {
        select: ['ID', 'TITLE'],
        filter: {
            TITLE: '%services%',
            SITE_ID: 205
        },
        order: {
            ID: 'DESC'
        }
    };
    
    try {
        const response = await $b24.callListMethod(
            'landing.landing.getList',
            { params, scope: 'knowledge' }
        );
        const items = response.getData() || [];
        for (const entity of items) {
            console.log('Entity:', entity);
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
        const generator = $b24.fetchListMethod('landing.landing.getList', { params, scope: 'knowledge' }, 'ID');
        for await (const page of generator) {
            for (const entity of page) {
                console.log('Entity:', entity);
            }
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
        const response = await $b24.callMethod('landing.landing.getList', { params, scope: 'knowledge' }, 0);
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
                    ],
                    'scope'  => 'knowledge'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
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
            },
            scope: 'knowledge'
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

{% include [Footnote on examples](../../_includes/examples.md) %}
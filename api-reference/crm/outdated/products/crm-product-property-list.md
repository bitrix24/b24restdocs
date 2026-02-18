# Get a list of product properties crm.product.property.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method development has been halted" %}

The method `crm.product.property.list` continues to function, but there is a more current equivalent [catalog.productProperty.list](../../../catalog/product-property/catalog-product-property-list.md).

{% endnote %}

The method `crm.product.property.list` returns a list of product properties.

## Method parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **order** | Sorting ||
|| **filter** | Filter fields ||
|#

## Code examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"PROPERTY_TYPE":"S","USER_TYPE":"HTML"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.property.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"PROPERTY_TYPE":"S","USER_TYPE":"HTML"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.product.property.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'crm.product.property.list',
        {
          order: { "SORT": "ASC" },
          filter: {
            "PROPERTY_TYPE": "S",
            "USER_TYPE": "HTML"
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
      const generator = $b24.fetchListMethod('crm.product.property.list', {
        order: { "SORT": "ASC" },
        filter: {
          "PROPERTY_TYPE": "S",
          "USER_TYPE": "HTML"
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
      const response = await $b24.callMethod('crm.product.property.list', {
        order: { "SORT": "ASC" },
        filter: {
          "PROPERTY_TYPE": "S",
          "USER_TYPE": "HTML"
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
                'crm.product.property.list',
                [
                    'order' => ['SORT' => 'ASC'],
                    'filter' => [
                        'PROPERTY_TYPE' => 'S',
                        'USER_TYPE'    => 'HTML'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        if ($response->more()) {
            $response->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching product properties: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.product.property.list", {
            order: {"SORT": "ASC"},
            filter: {
                "PROPERTY_TYPE": "S",
                "USER_TYPE": "HTML"
            }
        },
        function (result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.dir(result.data());
                if (result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.product.property.list',
        [
            'order' => ['SORT' => 'ASC'],
            'filter' => [
                'PROPERTY_TYPE' => 'S',
                'USER_TYPE' => 'HTML'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}


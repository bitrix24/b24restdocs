# Get a list of product properties crm.product.property.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method development has been halted" %}

The method `crm.product.property.list` continues to function, but there is a more relevant alternative [catalog.productProperty.list](../../../catalog/product-property/catalog-product-property-list.md).

{% endnote %}

The method `crm.product.property.list` returns a list of product properties.

## Method Parameters

{% include [Note about required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **order** | Sorting ||
|| **filter** | Filter fields ||
|#

## Code Examples

{% include [Note about examples](../../../../_includes/examples.md) %}

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

- PHP

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
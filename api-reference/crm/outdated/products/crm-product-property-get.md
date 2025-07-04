# Get product property by ID crm.product.property.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method development has been halted" %}

The method `crm.product.property.get` is still operational, but there is a more relevant alternative [catalog.productProperty.get](../../../catalog/product-property/catalog-product-property-get.md).

{% endnote %}

The method `crm.product.property.get` returns the property of products by ID.

## Method parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the product property ||
|#

## Code examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_product_id"}' \ # Replace 'your_product_id' with the actual product ID
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/crm.product.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_property_id"}' \ # Replace 'your_property_id' with the actual property ID
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/crm.product.property.get
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.product.property.get",
        {
            id: id
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $id = 'your_property_id'; // Replace 'your_property_id' with the actual property ID

    $result = CRest::call(
        'crm.product.property.get',
        [
            'id' => $id
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}
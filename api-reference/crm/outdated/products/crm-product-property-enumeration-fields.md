# Get the fields of the enumeration type property crm.product.property.enumeration.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method development has been halted" %}

The method `crm.product.property.enumeration.fields` is still operational, but there is a more relevant alternative [catalog.productPropertyEnum.*](../../../catalog/product-property-enum/index.md).

{% endnote %}

The method `crm.product.property.enumeration.fields` returns the description of the fields of the enumeration type product property.

No parameters.

## Code Examples

{% include [Note about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.property.enumeration.fields
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.product.property.enumeration.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.product.property.enumeration.fields",
        {},
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

    $result = CRest::call(
        'crm.product.property.enumeration.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}
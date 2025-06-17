# Get fields of additional settings for custom type crm.product.property.settings.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method development has been halted" %}

The method `crm.product.property.settings.fields` is still operational, but there is a more relevant alternative [catalog.productPropertyFeature.*](../../../catalog/product-property-feature/index.md).

{% endnote %}

The method `crm.product.property.settings.fields` returns a description of the fields for additional settings of custom type product properties.

## Method parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **propertyType** | Property type ||
|| **userType** | Custom property type ||
|#

## Code examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"propertyType":"S","userType":"HTML"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.property.settings.fields
   ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"propertyType":"S","userType":"HTML","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.product.property.settings.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.product.property.settings.fields",
        {propertyType: "S", userType: "HTML"},
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
        'crm.product.property.settings.fields',
        [
            'propertyType' => 'S',
            'userType' => 'HTML'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}
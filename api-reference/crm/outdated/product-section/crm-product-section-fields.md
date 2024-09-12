# Get Fields of the Product Section crm.productsection.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.productsection.fields` returns the description of the [fields of the section](./crm-product-section-add.md) for the product.

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.productsection.fields
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/crm.productsection.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.productsection.fields",
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
        'crm.productsection.fields',
        []
    );

    if (isset($result['error'])) {
        echo "Error: " . $result['error_description'] . "\n";
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

{% endlist %}

## Fields

#|
|| **Name**
`type`  | **Description** | **Note** ||
|| **CATALOG_ID** 
[`integer`](../../data-types.md) | Catalog Identifier | Immutable ||
|| **ID** 
[`integer`](../../data-types.md) | Section Identifier | Read-only ||
|| **NAME** 
[`string`](../../data-types.md) | Section Name | Required ||
|| **SECTION_ID** 
[`integer`](../../data-types.md) | Identifier of the linked section | ||
|| **XML_ID** 
[`string`](../../data-types.md) | Symbolic Code | ||
|#
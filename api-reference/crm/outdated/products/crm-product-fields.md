# Get Product Fields crm.product.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the description of product fields.

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.fields
   ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.product.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.product.fields",
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
        'crm.product.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Returned Data

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Active  ||
|| **CATALOG_ID**
[`integer`](../../../data-types.md) | Catalog identifier  ||
|| **CREATED_BY**
[`integer`](../../../data-types.md) | Created by  ||
|| **CURRENCY_ID**
[`string`](../../../data-types.md) | Currency identifier  ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Product creation date  ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description  ||
|| **DESCRIPTION_TYPE**
[`string`](../../../data-types.md) | Description type  ||
|| **DETAIL_PICTURE**
[`product_file`](../../../data-types.md) | Detail picture  ||
|| **ID**
[`integer`](../../../data-types.md) | Product identifier  ||
|| **MEASURE**
[`integer`](../../../data-types.md) | Unit of measurement  ||
|| **MODIFIED_BY**
[`integer`](../../../data-types.md) | Modified by  ||
|| **NAME***
[`string`](../../../data-types.md) | Name  ||
|| **PREVIEW_PICTURE**
[`product_file`](../../../data-types.md) | Preview picture  ||
|| **PRICE**
[`double`](../../../data-types.md) | Price  ||
|| **SECTION_ID**
[`integer`](../../../data-types.md) | Section identifier  ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting  ||
|| **TIMESTAMP_X**
[`datetime`](../../../data-types.md) | Product modification date  ||
|| **VAT_ID**
[`integer`](../../../data-types.md) | VAT rate identifier  ||
|| **VAT_INCLUDED**
[`char`](../../../data-types.md) | VAT included in price  ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code  ||
|#
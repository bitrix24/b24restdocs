# Get Product Fields crm.product.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method development has been halted" %}

The method `crm.product.fields` continues to function, but there are more relevant alternatives [catalog.product.*](../../../catalog/product/index.md).

{% endnote %}

The method `crm.product.fields` returns a description of the product fields.

Without parameters.

## Code Examples

{% include [Note about examples](../../../../_includes/examples.md) %}

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
    try
    {
        const response = await $b24.callMethod(
            "crm.product.fields",
            {}
        );
        
        const result = response.getData().result;
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            console.dir(result);
        }
    }
    catch(error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php        
    try {
        $fieldsResult = $serviceBuilder->getCRMScope()->product()->fields();
        $fieldsDescription = $fieldsResult->getFieldsDescription();
        foreach ($fieldsDescription as $field) {
            if (isset($field['DATE_CREATE'])) {
                $field['DATE_CREATE'] = (new DateTime($field['DATE_CREATE']))->format(DateTime::ATOM);
            }
            
            if (isset($field['TIMESTAMP_X'])) {
                $field['TIMESTAMP_X'] = (new DateTime($field['TIMESTAMP_X']))->format(DateTime::ATOM);
            }
            
            print($field['ID'] . ': ' . $field['NAME'] . PHP_EOL);
        }
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage() . PHP_EOL);
    }
    ```

- BX24.js

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

- PHP CRest

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

{% include [Note about required parameters](../../../../_includes/required.md) %}

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
[`product_file`](../../../data-types.md) | Detailed picture  ||
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
[`integer`](../../../data-types.md) | Sort order  ||
|| **TIMESTAMP_X**
[`datetime`](../../../data-types.md) | Product modification date  ||
|| **VAT_ID**
[`integer`](../../../data-types.md) | VAT rate identifier  ||
|| **VAT_INCLUDED**
[`char`](../../../data-types.md) | VAT included in price  ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code  ||
|#
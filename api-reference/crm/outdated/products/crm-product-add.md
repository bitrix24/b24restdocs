# Add Product crm.product.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method creates a new product.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`array`](../../../data-types.md) | Field values for creating a product.

To find out the required format of the fields, execute the method [crm.product.fields](./crm-product-fields.md) and check the format of the incoming values for these fields ||
|#

{% note info %}

Starting from version **CRM 21.700.0**, support for auto-generating the product's symbolic code is included, provided that the generation is enabled in the infoblock settings and no external service is used. The method `generateMnemonicCode` is utilized.

If the generated symbolic code exceeds 100 characters, it is automatically truncated to 100 characters. This should be taken into account when creating requests by passing a unique value at the beginning/middle of the product name to avoid duplicate symbolic codes.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"NAME":"Plastic Chair","CURRENCY_ID":"USD","PRICE":4900,"SORT":500}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"NAME":"Plastic Chair","CURRENCY_ID":"USD","PRICE":4900,"SORT":500},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.product.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.product.add",
        {
            fields:
            {
                "NAME": "Plastic Chair",
                "CURRENCY_ID": "USD",
                "PRICE": 4900,
                "SORT": 500
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Created new product with ID " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.product.add',
        [
            'fields' => [
                'NAME' => 'Plastic Chair',
                'CURRENCY_ID' => 'USD',
                'PRICE' => 4900,
                'SORT' => 500
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)

    ```php        
    try {
        $fields = [
            'NAME' => 'Sample Product',
            'PRICE' => '100.00',
            'CURRENCY_ID' => 'USD',
            'ACTIVE' => 'Y',
            'DATE_CREATE' => (new DateTime())->format(DateTime::ATOM),
            'TIMESTAMP_X' => (new DateTime())->format(DateTime::ATOM),
            'CREATED_BY' => 1,
            'MODIFIED_BY' => 1,
            'CATALOG_ID' => 1,
            'DESCRIPTION' => 'This is a sample product.',
            'VAT_ID' => 1,
            'VAT_INCLUDED' => 'Y',
            'MEASURE' => 1,
            'SECTION_ID' => 1,
            'SORT' => 100,
            'XML_ID' => 'sample_product_001',
        ];
        $result = $serviceBuilder->getCRMScope()->product()->add($fields);
        print($result->getId());
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

{% endlist %}
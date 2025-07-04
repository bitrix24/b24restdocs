# Get product by ID crm.product.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method development has been halted" %}

The method `crm.product.get` continues to function, but there are more relevant alternatives [catalog.product.*](../../../catalog/product/index.md).

{% endnote %}

The method `crm.product.get` returns a product by its ID.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Product ID ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_product_id"}' \ # Replace 'your_product_id' with the actual product ID
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_product_id","auth":"**put_access_token_here**"}' \ # Replace 'your_product_id' with the actual product ID
    https://**put_your_bitrix24_address**/rest/crm.product.get
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.product.get",
        { id: id },
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

    $id = 'your_product_id'; // Replace 'your_product_id' with the actual product ID

    $result = CRest::call(
        'crm.product.get',
        [
            'id' => $id
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)

    ```php        
    try {
        $productId = 1; // Example product ID
        $productService = $serviceBuilder->getCRMScope()->product();
        $productResult = $productService->get($productId);
        $itemResult = $productResult->product();
        
        print("ID: " . $itemResult->ID . "\n");
        print("Catalog ID: " . $itemResult->CATALOG_ID . "\n");
        print("Price: " . $itemResult->PRICE . "\n");
        print("Currency ID: " . $itemResult->CURRENCY_ID . "\n");
        print("Name: " . $itemResult->NAME . "\n");
        print("Code: " . $itemResult->CODE . "\n");
        print("Description: " . $itemResult->DESCRIPTION . "\n");
        print("Description Type: " . $itemResult->DESCRIPTION_TYPE . "\n");
        print("Active: " . $itemResult->ACTIVE . "\n");
        print("Section ID: " . $itemResult->SECTION_ID . "\n");
        print("Sort: " . $itemResult->SORT . "\n");
        print("VAT ID: " . $itemResult->VAT_ID . "\n");
        print("VAT Included: " . $itemResult->VAT_INCLUDED . "\n");
        print("Measure: " . $itemResult->MEASURE . "\n");
        print("XML ID: " . $itemResult->XML_ID . "\n");
        print("Preview Picture: " . $itemResult->PREVIEW_PICTURE . "\n");
        print("Detail Picture: " . $itemResult->DETAIL_PICTURE . "\n");
        print("Date Create: " . $itemResult->DATE_CREATE . "\n");
        print("Timestamp X: " . $itemResult->TIMESTAMP_X . "\n");
        print("Modified By: " . $itemResult->MODIFIED_BY . "\n");
        print("Created By: " . $itemResult->CREATED_BY . "\n");
    } catch (\Throwable $e) {
        print("Error: " . $e->getMessage() . "\n");
    }
    ```

{% endlist %}
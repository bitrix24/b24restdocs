# Get deal products crm.deal.productrows.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples (in other languages) are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.productrows.get` returns the product items of a deal.

The returned product items can be of the following types (field **TYPE**):

- `1` – simple product;
- `4` – trade offer/variation.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Deal identifier. ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.deal.productrows.get",
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

- B24-PHP-SDK
  
    ```php
    try {
        $dealId = 123; // Example deal ID
        $productRows = [
            [
                'ID' => 1,
                'OWNER_ID' => 123,
                'OWNER_TYPE' => 'D',
                'PRODUCT_ID' => 456,
                'PRODUCT_NAME' => 'Product 1',
                'PRICE' => '100.00',
                'PRICE_EXCLUSIVE' => '100.00',
                'PRICE_NETTO' => '100.00',
                'PRICE_BRUTTO' => '100.00',
                'QUANTITY' => '1',
                'DISCOUNT_TYPE_ID' => 1,
                'DISCOUNT_RATE' => '0',
                'DISCOUNT_SUM' => '0',
                'TAX_RATE' => '20',
                'TAX_INCLUDED' => 'Y',
                'CUSTOMIZED' => 'N',
                'MEASURE_CODE' => 1,
                'MEASURE_NAME' => 'pcs',
                'SORT' => 100,
            ],
            // Add more product rows as needed
        ];

        $result = $serviceBuilder
            ->getCRMScope()
            ->dealProductRows()
            ->set($dealId, $productRows);

        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print("Failed to set product rows.");
        }
    } catch (Throwable $e) {
        print("Error occurred: " . $e->getMessage());
    }
    ```

{% endlist %}    

{% include [Example notes](../../../_includes/examples.md) %}
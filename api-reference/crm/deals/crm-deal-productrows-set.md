# Add Products to Deal crm.deal.productrows.set

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples (in other languages) are missing
- success response is absent
- error response is absent
- link to the yet-to-be-created page is not provided

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.productrows.set` sets (creates or updates) the product items of a deal.

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the deal. ||
|| **rows** | Product items – an array of the form `array(array("field"=>"value"[, ...])[, ...])`, where "field" can take values from the method [crm.productrow.fields](.). The product items of the deal that exist before the method call will be replaced with the new ones. After saving, the deal amount will be recalculated. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.deal.productrows.set",
        {
            id: id,
            rows:
            [
                { "PRODUCT_ID": 689, "PRICE": 100.00, "QUANTITY": 4 },
                { "PRODUCT_ID": 690, "PRICE": 400.00, "QUANTITY": 1 }
            ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}
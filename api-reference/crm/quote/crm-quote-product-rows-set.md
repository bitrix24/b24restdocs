# Create/Update Product Items in crm.quote.productrows.set

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not specified
- examples missing (should include three examples - curl, js, php)
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.productrows.set` sets (creates or updates) the product items of the estimate.

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the estimate. ||
|| **rows**
[`unknown`](../../data-types.md) | Product items - an array of the form `array(array("field"=>"value"[, ...])[, ...])`, where "field" can take values from the returned method [crm.productrow.fields](../../crm/outdated/productrow-old/crm-productrow-fields.md). The product items of the estimate that exist before the method call will be replaced with the new ones. After saving, the total amount of the estimate will be recalculated. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.quote.productrows.set",
        {
            id: id,
            rows:
            [
                { "PRODUCT_ID": 1, "PRICE": 100.00, "QUANTITY": 2 },
                { "PRODUCT_ID": 2, "PRICE": 200.00, "QUANTITY": 1 }
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

{% include [Footnote on examples](../../../_includes/examples.md) %}
# Create/Update Product Items for crm.quote.productrows.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- revisions needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.productrows.set` sets (creates or updates) the product items of the quote.

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the quote. ||
|| **rows**
[`unknown`](../../data-types.md) | Product items - an array of the form `array(array("field"=>"value"[, ...])[, ...])`, where "field" can take values returned by the method [crm.productrow.fields](../../crm/outdated/productrow-old/crm-productrow-fields.md). The product items of the quote that exist before the method call will be replaced with the new ones. After saving, the total amount of the quote will be recalculated. ||
|#

## Example

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

{% include [Footnote on examples](../../../_includes/examples.md) %}
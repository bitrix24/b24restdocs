# Set Section Settings for Properties catalog.productPropertySection.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples in other languages are missing
- Response in case of success and error is absent

{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sets (creates or updates) the section settings for product properties or trade offers.

#|
|| **Parameter** | **Description** ||
|| **propertyId**
 `integer`  | Identifier of the product property or trade offer. ||
|| **fields**
`object` | An array containing the following fields:
- **smartFilter** – show in the smart filter (Y/N);
- **displayType** – type in the smart filter; possible values:
  - "F" – checkboxes;
  - "K" – radio buttons;
  - "P" – dropdown list.
- **displayExpanded** – show expanded (Y/N);
- **filterHint** – hint in the smart filter for visitors. ||
|#

## Example

```js
BX24.callMethod(
    'catalog.productPropertySection.set',
    {
        propertyId: 128,
        fields: {
            displayType: "F",
            displayExpanded: "Y",
            filterHint: "Product Size"
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```

{% include [Footnote on examples](../../../_includes/examples.md) %}
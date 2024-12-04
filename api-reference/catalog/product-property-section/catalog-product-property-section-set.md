# Set Section Settings for catalog.productPropertySection.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples in other languages are missing
- response in case of success and error is missing

{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sets (creates or updates) the section settings for product properties or variations.

#|
|| **Parameter** | **Description** ||
|| **propertyId**
 `integer`  | Identifier of the product property or variation. ||
|| **fields**
`object` | An array containing the following fields:
- **smartFilter** – show in smart filter (Y/N);
- **displayType** – view in smart filter; possible values:
  - "F" – checkboxes;
  - "K" – radio buttons;
  - "P" – dropdown list.
- **displayExpanded** – show expanded (Y/N);
- **filterHint** – hint in the smart filter for visitors. ||
|#

## Example

{% list tabs %}

- JS

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

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}
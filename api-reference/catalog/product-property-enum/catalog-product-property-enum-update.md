# Update Values of List Properties catalog.productPropertyEnum.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- No response in case of error
- No response in case of success
- No examples in other languages

{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.productPropertyEnum.update(id, fields)
```

Method for updating values of list properties.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the product property or trade offer. ||
|| **fields** 
[`object`](../../data-types.md)|  Fields corresponding to the available list of fields [`fields`](catalog-product-property-enum-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.productPropertyEnum.update',
    {
        id: 122,
        fields: {
            propertyId: 128,
            value: "Medium",
            def: "Y",
            sort: 1234,
            xmlId: "M"
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
{% include [Note on examples](../../../_includes/examples.md) %}
# Update Price Rounding Rule catalog.roundingRule.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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
catalog.roundingRule.update(id, fields)
```

Method for updating the fields of the price rounding rule.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the price rounding rule. ||
|| **fields** 
[`object`](../../data-types.md)|  Fields corresponding to the available list of fields [`fields`](catalog-rounding-rule-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.roundingRule.update',
    {
        id: 10,
        fields: {
            catalogGroupId: 14,
            price: 1000,
            roundType: 4,
            roundPrecision: 10,
        }
    },
    function(result) {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Note on examples](../../../_includes/examples.md) %}
# Get a List of Price Rounding Rules by Filter catalog.roundingRule.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- The mandatory nature of parameters is not specified
- There is no response in case of an error
- No examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
catalog.roundingRule.list(select, filter, order, start)
```

This method retrieves a list of price rounding rules based on the filter.

If the operation is successful, a list of prices is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **select** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-rounding-rule-get-fields.md).||
|| **filter** 
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-rounding-rule-get-fields.md). ||
|| **order**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-rounding-rule-get-fields.md). ||
|| **start** 
[`string`](../../data-types.md)| Page number for output. Works for HTTPS requests. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

For JS

```javascript
BX24.callMethod(
    'catalog.roundingrule.list',
    {
        select: {
            id
        },
        filter: {
            modifiedBy: 1
        },
        order: {
            id: 'ASC'
        },
        start: 1
    },
    function(result) {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
        result.next();
    }
);
```

Example HTTPS request

```
https://your_account/rest/catalog.roundingRule.list?auth=_authorization_key_&start=50
```

{% include [Examples Note](../../../_includes/examples.md) %}
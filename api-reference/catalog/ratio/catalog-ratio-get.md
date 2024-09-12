# Get Unit Ratio Field Values catalog.ratio.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.ratio.get(id)
```

Method to access the unit ratio field values by ID.

If the operation is successful, a [unit ratio resource](resource.md) is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id^*^** 
[`string`](../../data-types.md)| The unit ratio number. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.ratio.get',
    {
        id: 7
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
{% include [Examples Note](../../../_includes/examples.md) %}
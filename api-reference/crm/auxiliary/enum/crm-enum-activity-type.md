# Get Enumeration Elements "Activity Type" crm.enum.activitytype

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing response in case of error
- missing response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
crm.enum.activitytype()
```

Returns the enumeration elements "Activity Type".

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    "crm.enum.activitytype",
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}
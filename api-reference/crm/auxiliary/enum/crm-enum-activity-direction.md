# Get Enumeration Elements "Activity Direction" crm.enum.activitydirection

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
crm.enum.activitydirection()
```

Returns the enumeration elements "Activity Direction" (for e-mails and calls). Values: 1 - incoming, 2 - outgoing.

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    "crm.enum.activitydirection",
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
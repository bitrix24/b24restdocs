# Get VAT Rate Fields crm.vat.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- no examples in other languages
- no description of returned fields
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
crm.vat.fields()
```

The method returns the description of VAT rate fields.

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    "crm.vat.fields",
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
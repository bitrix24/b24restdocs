# Get Description of CRM Enumeration Fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

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
crm.enum.fields()
```

The method returns the description of the enumeration fields.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        "crm.enum.fields",
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

{% endlist %}


{% include [Footnote about examples](../../../../_includes/examples.md) %}
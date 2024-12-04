# Get Enumeration Items "Status" crm.enum.activitystatus

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

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
crm.enum.activitystatus()
```

Returns the enumeration items "Status" (STATUS).

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        "crm.enum.activitystatus",
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
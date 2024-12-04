# Get Enumeration Items "Activity Notification Type" crm.enum.activitynotifytype

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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
crm.enum.activitynotifytype()
```

Returns the enumeration items "Activity Notification Type" (for meetings and calls).

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS
    
    ```javascript
    BX24.callMethod(
        "crm.enum.activitynotifytype",
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


{% include [Footnote on examples](../../../../_includes/examples.md) %}
# Get a List of Custom Activity Types crm.activity.type.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.type.list` retrieves a list of activity subtypes.

## Parameters

No parameters

## Example

```js
BX24.callMethod(
    'crm.activity.type.list',
    {
    },
    function(result)
    {
        if(result.error())
            alert("Error: " + result.error());
        else
        {
            console.log(result.data());
        }
    }
);
```

{% include [Footnote on examples](../../../../../_includes/examples.md) %}
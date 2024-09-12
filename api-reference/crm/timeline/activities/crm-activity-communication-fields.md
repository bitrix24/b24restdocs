# Get Description of Communication crm.activity.communication.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.communication.fields` returns the description of communication for an activity. Communications store phone numbers in calls, email addresses in e-mails, and names in meetings.

## Parameters

No parameters

## Examples

```js
BX24.callMethod(
    "crm.activity.communication.fields",
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

{% include [Note on examples](../../../../_includes/examples.md) %}
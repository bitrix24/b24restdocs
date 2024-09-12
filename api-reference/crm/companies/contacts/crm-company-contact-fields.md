# Get Field Descriptions for Company-Contact Connection crm.company.contact.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

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

The method `crm.company.contact.fields` returns the field descriptions for the company-contact connection, used by methods in the `crm.company.contact.*` family, such as `crm.company.contact.items.get`, `crm.company.contact.items.set`, `crm.company.contact.add`, etc.

## Parameters

No parameters.

## Examples

```js
BX24.callMethod(
    "crm.company.contact.fields",
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

{% include [Example Note](../../../../_includes/examples.md) %}
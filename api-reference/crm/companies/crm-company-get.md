# Get Company Information crm.company.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.get` returns a [company](./crm-company-add.md) by its identifier.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Company identifier. ||
|#

## Examples

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.company.get",
    { id: id },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```

{% include [Example Note](../../../_includes/examples.md) %}
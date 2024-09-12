# Delete Custom Field for Companies crm.company.userfield.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.userfield.delete` removes a custom field for companies.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Identifier of the custom field. ||
|#

## Examples

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.company.userfield.delete",
    {
        id: id
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
);
```

{% include [Example Notes](../../../../_includes/examples.md) %}
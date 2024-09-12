# Get Custom Company Field by ID crm.company.userfield.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

`crm.company.userfield.get(id)`

The method `crm.company.userfield.get` returns a custom company field by its ID.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | The identifier of the custom field. ||
|#

## Example

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.company.userfield.get",
    {
        id: id
    },
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
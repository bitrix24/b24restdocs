# Delete Field crm.lead.userfield.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples (in other languages) are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.userfield.delete` removes a custom field from leads.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the custom field. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Example

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.lead.userfield.delete",
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

{% include [Examples Note](../../../../_includes/examples.md) %}
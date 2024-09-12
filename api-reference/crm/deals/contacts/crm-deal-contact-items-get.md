# Get the set of contacts associated with the deal crm.deal.contact.items.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing (in other languages)
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.contact.items.get` returns a set of contacts associated with the specified deal.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the deal. ||
|#

{% include [Parameter notes](../../../../_includes/required.md) %}

## Example

```js
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.deal.contact.items.get",
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

{% include [Example notes](../../../../_includes/examples.md) %}

## Response on success

The result is returned as an array of objects, each containing the following fields:

#|
|| **Field** | **Description** ||
|| **CONTACT_ID** | Identifier of the contact ||
|| **SORT** | Sort index ||
|| **ROLE_ID** | Identifier of the role (reserved) ||
|| **IS_PRIMARY** | Primary contact flag ||
|#
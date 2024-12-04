# Get a set of contacts associated with the specified company crm.company.contact.items.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- parameter types not specified
- parameter requirements not specified
- examples missing
- success response missing
- error response missing
- links to yet-to-be-created pages not provided

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.contact.items.get` returns a set of contacts associated with the specified company.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Company identifier. ||
|#

The result is returned as an array of objects, each containing the following fields:

#|
|| **Field** | **Description** ||
|| **CONTACT_ID**
[`unknown`](../../../data-types.md) | Contact identifier. ||
|| **SORT**
[`unknown`](../../../data-types.md) | Sort index. ||
|| **ROLE_ID**
[`unknown`](../../../data-types.md) | Role identifier (reserved). ||
|| **IS_PRIMARY**
[`unknown`](../../../data-types.md) | Primary contact flag. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.company.contact.items.get",
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

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}
# Get Invoice Status Fields crm.invoice.status.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.invoice.status.fields` returns the description of the invoice status fields.

{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.fields](../../../crm/status/crm-status-fields.md)

{% endnote %}

Without parameters

### Returned Data

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Identifier of the entity associated with the invoice. Read-only ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the invoice. Read-only  ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the section  ||
|| **NAME_INIT**
[`string`](../../../data-types.md) | Read-only  ||
|| **SORT***
[`integer`](../../../data-types.md) | Sorting  ||
|| **STATUS_ID**
[`string`](../../../data-types.md) | Status. Read-only  ||
|| **SYSTEM**
[`char`](../../../data-types.md) | Whether it is system or not. Read-only ||
|#
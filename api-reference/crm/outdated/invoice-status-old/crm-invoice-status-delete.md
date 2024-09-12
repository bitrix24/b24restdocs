# Delete Invoice Status crm.invoice.status.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.invoice.status.delete` removes the invoice status.

{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.delete](../../../crm/status/crm-status-delete.md)

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the invoice status ||
|#
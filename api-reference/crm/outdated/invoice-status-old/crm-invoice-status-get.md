# Get Invoice Status by ID crm.invoice.status.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.invoice.status.get` returns the status of an invoice by its ID.

{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.get](../../../crm/status/crm-status-get.md)

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | The ID of the invoice status || 
|#
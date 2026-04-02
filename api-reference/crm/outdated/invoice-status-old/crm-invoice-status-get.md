# Get Invoice Status by ID `crm.invoice.status.get`

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute this method: any user

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

The method `crm.invoice.status.get` returns the status of an invoice based on its ID.

{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.get](../../../crm/status/crm-status-get.md).

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | The identifier of the invoice status || 
|#
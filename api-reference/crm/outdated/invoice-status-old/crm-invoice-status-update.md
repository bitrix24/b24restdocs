# Update Invoice Status crm.invoice.status.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

The method `crm.invoice.status.update` returns the status of an invoice by its identifier.

{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.update](../../../crm/status/crm-status-update.md).

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the invoice status ||
|| **fields***
[`array`](../../data-types.md) | Set of fields — an array in the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values returned by the method [crm.invoice.status.fields](./crm-invoice-status-fields.md). 

{% note info %}

To find out the required format for the fields, execute the method [crm.invoice.status.fields](./crm-invoice-status-fields.md) and check the format of the returned values for these fields.

{% endnote %}
||
|#
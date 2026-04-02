# Create a New Invoice Status crm.invoice.status.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

The method `crm.invoice.status.add` creates a new invoice status.

{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.add](../../../crm/status/crm-status-add.md).

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`array`](../../data-types.md) | A set of fields — an array in the form `array("field"=>"value"[, ...])`, containing the values of the invoice status fields. 

{% note info %}

To find out the required format for the fields, execute the method [crm.invoice.status.fields](./crm-invoice-status-fields.md) and check the format of the returned values for these fields.

{% endnote %}

||
|#
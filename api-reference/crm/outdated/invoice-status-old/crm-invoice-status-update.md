# Update Invoice Status crm.invoice.status.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.invoice.status.update` returns the status of an invoice by its identifier.

{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.update](../../../crm/status/crm-status-update.md)

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the invoice status ||
|| **fields***
[`array`](../../data-types.md) | Set of fields — an array in the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values from the method [crm.invoice.status.fields](./crm-invoice-status-fields.md). 

{% note info %}

To find out the required format of the fields, execute the method [crm.invoice.status.fields](./crm-invoice-status-fields.md) and check the format of the returned values for these fields.

{% endnote %}
|| 
|#
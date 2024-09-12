# Event for Issuing a New Invoice from Regular onCrmInvoiceRecurringExpose

The event is triggered when a new invoice is issued from a recurring invoice.

## Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS** 
[`array`](../../../../data-types.md)| The array contains the following fields:
- **ID** — the identifier value of the record in the recurring invoice settings table
- **RECURRING_INVOICE_ID** — the identifier value of the recurring invoice template
- **INVOICE_ID** — the ID value of the new invoice created based on the recurring invoice || 
|#
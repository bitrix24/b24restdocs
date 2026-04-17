# Old Invoices: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods `crm.invoice.*` has been halted. 
Please use the section [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Methods

#| 
|| **Method** | **Description** ||
|| [crm.invoice.add](./crm-invoice-add.md) | Creates a new invoice ||
|| [crm.invoice.delete](./crm-invoice-delete.md) | Deletes an invoice ||
|| [crm.invoice.fields](./crm-invoice-fields.md) | Returns the description of invoice fields and items included in it ||
|| [crm.invoice.get](./crm-invoice-get.md) | Returns an invoice by its ID ||
|| [crm.invoice.list](./crm-invoice-list.md) | Returns a list of invoices ||
|| [crm.invoice.recurring.add](./crm-invoice-recurring-add.md) | Adds a new setting for a recurring invoice ||
|| [crm.invoice.recurring.delete](./crm-invoice-recurring-delete.md) | Deletes an existing setting for a recurring invoice template ||
|| [crm.invoice.recurring.expose](./crm-invoice-recurring-expose.md) | Creates a new invoice from a template ||
|| [crm.invoice.recurring.fields](./crm-invoice-recurring-fields.md) | Returns a list of fields for the recurring invoice template setting with descriptions ||
|| [crm.invoice.recurring.get](./crm-invoice-recurring-get.md) | Returns the fields of the recurring invoice template setting by ID ||
|| [crm.invoice.recurring.list](./crm-invoice-recurring-list.md) | Returns a list of recurring invoice template settings based on a filter ||
|| [crm.invoice.recurring.update](./crm-invoice-recurring-update.md) | Updates an existing setting for a recurring invoice template ||
|| [crm.invoice.update](./crm-invoice-update.md) | Updates an existing invoice ||
|| [crm.invoice.userfield.add](./crm-invoice-user-field-add.md) | Creates a new custom field for invoices ||
|| [crm.invoice.userfield.delete](./crm-invoice-user-field-delete.md) | Deletes a custom field for invoices ||
|| [crm.invoice.userfield.get](./crm-invoice-user-field-get.md) | Returns a custom field for invoices by ID ||
|| [crm.invoice.userfield.list](./crm-invoice-user-field-list.md) | Returns a list of custom fields for invoices based on a filter ||
|| [crm.invoice.userfield.update](./crm-invoice-user-field-update.md) | Updates an existing custom field for invoices ||
|| [crm.paysystem.fields](./crm-pay-system-fields.md) | Returns the description of fields for payment methods ||
|| [crm.paysystem.list](./crm-pay-system-list.md) | Returns a list of payment methods ||
|| [crm.persontype.fields](./crm-person-type-fields.md) | Returns the description of fields for payer types ||
|| [crm.persontype.list](./crm-person-type-list.md) | Returns a list of payer types ||
|| [crm.invoice.getexternallink](./crm-invoice-get-external-link.md) | Returns a link to the invoice ||
|#

## Events

#| 
|| **Event** | **Triggered** ||
|| [onCrmInvoiceAdd](./events-invoice/on-crm-invoice-add.md) | When an invoice is created ||
|| [onCrmInvoiceDelete](./events-invoice/on-crm-invoice-delete.md) | When an invoice is deleted ||
|| [onCrmInvoiceSetStatus](./events-invoice/on-crm-invoice-set-status.md) | When the status of an invoice is changed ||
|| [onCrmInvoiceUpdate](./events-invoice/on-crm-invoice-update.md) | When an invoice is updated ||
|| [onCrmInvoiceUserFieldAdd](./events-invoice/on-crm-invoice-user-field-add.md) | When a custom field is added ||
|| [onCrmInvoiceUserFieldUpdate](./events-invoice/on-crm-invoice-user-field-update.md) | When a custom field is modified ||
|| [onCrmInvoiceUserFieldDelete](./events-invoice/on-crm-invoice-recurring-delete.md) | When a custom field is deleted ||
|| [onCrmInvoiceUserFieldSetEnumValues](./events-invoice/on-crm-invoice-user-field-set-enum-values.md) | When the set of values for a custom field of list type is changed ||
|| [onCrmInvoiceRecurringAdd](./events-invoice/on-crm-invoice-recurring-add.md) | When a new recurring invoice is created ||
|| [onCrmInvoiceRecurringUpdate](./events-invoice/on-crm-invoice-recurring-update.md) | When a recurring invoice is updated ||
|| [onCrmInvoiceRecurringDelete](./events-invoice/on-crm-invoice-recurring-delete.md) | When a recurring invoice is deleted ||
|| [onCrmInvoiceRecurringExpose](./events-invoice/on-crm-invoice-recurring-expose.md) | When a new invoice is issued from a recurring invoice ||
|#
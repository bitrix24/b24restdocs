# Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "Access" %}

**Scope**: [`crm`](../../../../scopes/permissions.md) | **Who can subscribe**: `any user`

{% endnote %}

#| 
|| **Event** | **Triggered** ||
|| [onCrmInvoiceAdd](./on-crm-invoice-add.md) | when an invoice is created ||
|| [onCrmInvoiceDelete](./on-crm-invoice-delete.md) | when an invoice is deleted ||
|| [onCrmInvoiceSetStatus](./on-crm-invoice-set-status.md) | when the status of an invoice is changed ||
|| [onCrmInvoiceUpdate](./on-crm-invoice-update.md) | when an invoice is updated ||
|| [onCrmInvoiceUserFieldAdd](./on-crm-invoice-user-field-add.md) | when a custom field is added ||
|| [onCrmInvoiceUserFieldUpdate](./on-crm-invoice-user-field-update.md) | when a custom field is modified ||
|| [onCrmInvoiceUserFieldDelete](./on-crm-invoice-recurring-delete.md) | when a custom field is deleted ||
|| [onCrmInvoiceUserFieldSetEnumValues](./on-crm-invoice-user-field-set-enum-values.md) | when the set of values for a custom list-type field is changed ||
|| [onCrmInvoiceRecurringAdd](./on-crm-invoice-recurring-add.md) | when a new recurring invoice is created ||
|| [onCrmInvoiceRecurringUpdate](./on-crm-invoice-recurring-update.md) | when a recurring invoice is updated ||
|| [onCrmInvoiceRecurringDelete](./on-crm-invoice-recurring-delete.md) | when a recurring invoice is deleted ||
|| [onCrmInvoiceRecurringExpose](./on-crm-invoice-recurring-expose.md) | when a new invoice is issued from a recurring invoice ||
|#
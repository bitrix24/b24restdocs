# Deprecated Payment System Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The methods in this section support integration with legacy CRM invoices. Development of the methods [crm.invoice.*](../../crm/outdated/invoice/index.md) has been halted.

Please use the section [Universal methods for invoices](../../crm/universal/invoice.md).

{% endnote %}

> Scope: [`pay_system`](../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

## Methods

#|
|| **Method** | **Description** ||
|| [sale.paysystem.pay.invoice](./sale-pay-system-pay-invoice.md) | Initiates payment for an invoice through a specific payment system ||
|| [sale.paysystem.settings.invoice.get](./sale-pay-system-settings-invoice-get.md) | Returns payment system settings for a specific invoice ||
|#
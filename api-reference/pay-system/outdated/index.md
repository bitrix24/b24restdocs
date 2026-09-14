# Deprecated Payment System Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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
# Cash Registers in Online Stores: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

When you sell something through CRM, the purchase information goes to the cash register. The cash register generates a receipt and sends it to:
- the fiscal data operator for reporting to the tax authorities,
- the customer via SMS or e-mail.

To add a new cash register, first create a cash register handler. It will connect Bitrix24 with your cash register equipment or online service. Then add and configure the cash register: specify the name, select the handler, and set up fiscalization.

> Quick navigation: [all methods](#all-methods)

{% note tip "Typical use-cases and scenarios" %}

- [How to Connect a Cash Register to Bitrix24](../../../tutorials/sale/cashbox-add-example.md)

{% endnote %}

## How to Get Started

1. Create a cash register handler using [sale.cashbox.handler.add](./sale-cashbox-handler-add.md).
2. Retrieve the list of handlers using [sale.cashbox.handler.list](./sale-cashbox-handler-list.md) to select the handler code.
3. Add a cash register using [sale.cashbox.add](./sale-cashbox-add.md) and pass the fiscalization settings.
4. Check the list of cash registers using [sale.cashbox.list](./sale-cashbox-list.md).
5. After printing a receipt, pass the result using [sale.cashbox.check.apply](./sale-cashbox-check-apply.md).

## Overview of Methods {#all-methods}

> Scope: [`cashbox`](../../scopes/permissions.md)
>
> Who can perform the methods: CRM administrator with the permission "Allow to change settings"

### Cash Register Handlers

#| 
|| **Method** | **Description** ||
|| [sale.cashbox.handler.add](./sale-cashbox-handler-add.md) | Adds a cash register handler ||
|| [sale.cashbox.handler.update](./sale-cashbox-handler-update.md) | Updates the cash register handler data ||
|| [sale.cashbox.handler.list](./sale-cashbox-handler-list.md) | Returns a list of available cash register handlers ||
|| [sale.cashbox.handler.delete](./sale-cashbox-handler-delete.md) | Deletes a cash register handler ||
|#

### Cash Registers

#| 
|| **Method** | **Description** ||
|| [sale.cashbox.add](./sale-cashbox-add.md) | Adds a cash register ||
|| [sale.cashbox.update](./sale-cashbox-update.md) | Updates an existing cash register ||
|| [sale.cashbox.list](./sale-cashbox-list.md) | Returns a list of configured cash registers ||
|| [sale.cashbox.delete](./sale-cashbox-delete.md) | Deletes a cash register ||
|#

### Receipts

#| 
|| **Method** | **Description** ||
|| [sale.cashbox.check.apply](./sale-cashbox-check-apply.md) | Saves the result of receipt printing ||
|#
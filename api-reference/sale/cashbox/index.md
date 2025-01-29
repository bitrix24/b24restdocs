# Cash Registers in Online Stores: Overview of Methods

When you sell something through CRM, the purchase information goes to the cash register. The cash register generates a receipt and sends it to:
- the fiscal data operator for reporting to the tax authorities,
- the customer via SMS or e-mail.

To add a new cash register, first create a cash register handler. It will connect Bitrix24 with your cash register equipment or online service. Then add and configure the cash register: specify the name, select the handler, and set up fiscalization.

> Quick navigation: [all methods](#all-methods)

{% note tip "Typical use-cases and scenarios" %}

- [Implement a simple cash register using REST API](../../../tutorials/sale/cashbox-add-example.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
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
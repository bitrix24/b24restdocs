# Overview of Methods

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- where cash registers are used in Bitrix24
- how the exchange of receipts occurs
- where receipts are used
- what the interaction logic is between cash register handlers, cash registers, and receipts in the REST API

{% endnote %}

{% endif %}

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
>
> Who can execute the methods: CRM administrator (permission "Allow changing settings")

#|
|| **Method** | **Description** ||
|| [sale.cashbox.handler.add](./sale-cashbox-handler-add.md) | Adds a REST handler for the cash register ||
|| [sale.cashbox.handler.update](./sale-cashbox-handler-update.md) | Updates the data of the REST handler for the cash register ||
|| [sale.cashbox.handler.list](./sale-cashbox-handler-list.md) | Returns a list of available REST handlers for cash registers ||
|| [sale.cashbox.handler.delete](./sale-cashbox-handler-delete.md) | Deletes a REST handler for the cash register ||
|| [sale.cashbox.add](./sale-cashbox-add.md) | Adds a cash register ||
|| [sale.cashbox.update](./sale-cashbox-update.md) | Updates an existing cash register ||
|| [sale.cashbox.list](./sale-cashbox-list.md) | Returns a list of configured cash registers ||
|| [sale.cashbox.delete](./sale-cashbox-delete.md) | Deletes a cash register ||
|| [sale.cashbox.check.apply](./sale-cashbox-check-apply.md) | Saves the result of printing a receipt printed on a REST cash register ||
|#
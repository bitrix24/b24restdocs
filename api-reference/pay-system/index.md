# Payment Systems: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Payment systems accept payments from customers. For example, you can integrate bank acquiring or an online payment service. The customer selects a payment method during the checkout process, after which the system processes the transaction.

This section consolidates methods for registering a handler, creating a payment system, configuring parameters, and initiating payments.

> Quick navigation: [all methods](#all-methods)

## Connection with Other Objects

The payment system operates in conjunction with the payer type, payment, order, and handler. These connections determine the availability of the system and the external service through which the payment is processed.

**Payer Type.** The parameter `PERSON_TYPE_ID` defines which type of payer the payment system operates for.

**Payment.** The parameters `PAYMENT_ID` and `PAY_SYSTEM_ID` link the payment to a specific payment system. The payment settings are returned by the method [sale.paysystem.settings.payment.get](./sale-pay-system-settings-payment-get.md), and the payment is initiated by [sale.paysystem.pay.payment](./sale-pay-system-pay-payment.md).

**Order.** In the CRM invoice scenario, payment is executed through the associated order at the API level. When paying the invoice, the system changes the payment status of that order. The link between the invoice and the order is returned by the method [crm.orderentity.list](../crm/universal/order-entity/crm-order-entity-list.md).

**REST Handler.** The code `BX_REST_HANDLER` connects the payment system with an external payment provider.

## How to Get Started

1. Obtain the payer type identifier `PERSON_TYPE_ID` using the method [sale.persontype.list](../sale/person-type/sale-person-type-list.md).
2. Register the REST handler using the method [sale.paysystem.handler.add](./sale-pay-system-handler-add.md).
3. Create the payment system using the method [sale.paysystem.add](./sale-pay-system-add.md). Pass `PERSON_TYPE_ID` and the handler code `BX_REST_HANDLER`.
4. Check the result using the method [sale.paysystem.list](./sale-pay-system-list.md). If necessary, update the data using the method [sale.paysystem.update](./sale-pay-system-update.md).
5. Configure the operational parameters using the methods [sale.paysystem.settings.get](./sale-pay-system-settings-get.md) and [sale.paysystem.settings.update](./sale-pay-system-settings-update.md).
6. To process the order payment, obtain `PAYMENT_ID` using the method [sale.payment.list](../sale/payment/sale-payment-list.md), then initiate the payment through [sale.paysystem.pay.payment](./sale-pay-system-pay-payment.md).
7. For the CRM invoice, use a separate scenario: obtain `orderId` via [crm.orderentity.list](../crm/universal/order-entity/crm-order-entity-list.md), then find `PAYMENT_ID` in [sale.payment.list](../sale/payment/sale-payment-list.md) with the filter `{"=orderId": <orderId>}` and pass this identifier to [crm.item.payment.pay](../crm/universal/payment/crm-item-payment-pay.md).

## Overview of Methods {#all-methods}

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

### REST Handlers for Payment Systems

#|
|| **Method** | **Description** ||
|| [sale.paysystem.handler.add](./sale-pay-system-handler-add.md) | Adds a REST handler for the payment system ||
|| [sale.paysystem.handler.update](./sale-pay-system-handler-update.md) | Updates a REST handler for the payment system ||
|| [sale.paysystem.handler.list](./sale-pay-system-handler-list.md) | Returns a list of REST handlers for the payment system ||
|| [sale.paysystem.handler.delete](./sale-pay-system-handler-delete.md) | Deletes a REST handler for the payment system ||
|#

### Payment Systems

#|
|| **Method** | **Description** ||
|| [sale.paysystem.add](./sale-pay-system-add.md) | Adds a payment system ||
|| [sale.paysystem.update](./sale-pay-system-update.md) | Modifies a payment system ||
|| [sale.paysystem.list](./sale-pay-system-list.md) | Returns a list of payment systems ||
|| [sale.paysystem.delete](./sale-pay-system-delete.md) | Deletes a payment system ||
|| [sale.paysystem.settings.get](./sale-pay-system-settings-get.md) | Returns settings for the payment system ||
|| [sale.paysystem.settings.update](./sale-pay-system-settings-update.md) | Updates settings for the payment system ||
|| [sale.paysystem.settings.payment.get](./sale-pay-system-settings-payment-get.md) | Returns settings for the payment system for a specific payment ||
|| [sale.paysystem.pay.payment](./sale-pay-system-pay-payment.md) | Initiates payment for an order through the selected payment system ||
|#
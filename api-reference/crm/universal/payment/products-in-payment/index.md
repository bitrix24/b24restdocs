# Product Items in Payment: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Product items in a payment are products from the product rows of a deal or invoice that are included in the payment. Each item specifies the quantity being paid for, while the total amount is calculated based on product quantities and prices.

The methods `crm.item.payment.product.*` allow you to add a product item to a payment, retrieve the payment contents, change a product quantity, or delete an item.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Accept payment in the deal form](https://helpdesk.bitrix24.com/open/23570150/)

## How to Start

1. Prepare the `paymentId` of the desired payment using the main methods [crm.item.payment.*](../index.md).
2. Obtain the `rowId` of the desired product using the method [crm.item.productrow.list](../../product-rows/crm-item-productrow-list.md), or a list of products that have not been paid for yet using the method [crm.item.productrow.getAvailableForPayment](../../product-rows/crm-item-productrow-get-available-for-payment.md).
3. Add a product item using the method [crm.item.payment.product.add](./crm-item-payment-product-add.md).
4. Check the composition of the items using the method [crm.item.payment.product.list](./crm-item-payment-product-list.md).
5. If necessary, adjust the quantity using the method [crm.item.payment.product.setQuantity](./crm-item-payment-product-set-quantity.md) or remove the item using the method [crm.item.payment.product.delete](./crm-item-payment-product-delete.md).

## Product Allocation Restrictions Across Payments

The methods take into account the product quantity in the original product row and the quantity already allocated across all order payments.

- In `crm.item.payment.product.add`, the `rowId` parameter must refer to a product row in the same CRM object as the payment. If the product row belongs to another object, the method returns the `Product not found` error
- If the entire quantity in the product row has already been allocated among payments, `crm.item.payment.product.add` returns the `Product not found` error. If the product has been partially allocated, you can add no more than the available remainder; otherwise, the method returns the `Insufficient product quantity to add to payment` error
- The method `crm.item.payment.product.setQuantity` checks the total product quantity across all order payments. If the new item quantity exceeds the available remainder after accounting for other payments, the method returns the `Insufficient product quantity to add to payment` error and does not update the item

## Linking Product Items in Payment with Other Objects

**CRM Payment.** All methods in this group are executed in the context of a payment identified by `paymentId`.

**Product Catalog.** Serves as the source of information about the product. Data from the catalog is transferred to the CRM product line and is then used when forming the item in the payment.

**CRM Product Line.** The methods in this group work with the product line through `rowId`. The product line contains data about the product: identifier, name, quantity, price, unit of measurement, and other parameters. You can obtain `rowId` using the method [crm.item.productrow.list](../../product-rows/crm-item-productrow-list.md). Within the payment, you can only manage the quantity of the product.

**CRM Object.** A payment always belongs to a deal or an invoice — only these objects support payments. Therefore, product items in a payment exist only for them.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#| 
|| **Method** | **Description** ||
|| [crm.item.payment.product.add](./crm-item-payment-product-add.md) | Adds a product item to the payment ||
|| [crm.item.payment.product.list](./crm-item-payment-product-list.md) | Returns a list of product items in the payment ||
|| [crm.item.payment.product.delete](./crm-item-payment-product-delete.md) | Removes a product item from the payment ||
|| [crm.item.payment.product.setQuantity](./crm-item-payment-product-set-quantity.md) | Changes the quantity of the product in the payment item ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../../product-rows/index.md)
- [{#T}](../delivery-in-payment/index.md)

# Product Items in Payment: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods `crm.item.payment.product.*` manage product items within CRM payments. For example, you can add a specific product to a payment, change its quantity, or remove it from the payment.

This is necessary to detail the composition of the order in financial documents. Bitrix24 links products from the catalog with payments, allowing for accurate accounting of items during transactions.

> Quick navigation: [all methods](#all-methods)

## Linking Product Items in Payment with Other Objects

**CRM Payment.** All methods in this group are executed in the context of a payment identified by `paymentId`.

**Product Catalog.** Serves as the source of information about the product. Data from the catalog is transferred to the CRM product line and is then used when forming the item in the payment.

**CRM Product Line.** The methods in this group work with the product line through `rowId`. The product line contains data about the product: identifier, name, quantity, price, unit of measurement, and other parameters. You can obtain `rowId` using the method [crm.item.productrow.list](../../product-rows/crm-item-productrow-list.md). Within the payment, you can only manage the quantity of the product.

**CRM Object.** A payment always belongs to a deal or an invoice — only these objects support payments. Therefore, product items in a payment exist only for them.

## How to Work with Product Items in Payment

1. Prepare the `paymentId` of the desired payment using the main methods [crm.item.payment.*](../index.md).
2. Obtain the `rowId` of the desired product using the method [crm.item.productrow.list](../../product-rows/crm-item-productrow-list.md), or a list of products that have not been paid for yet using the method [crm.item.productrow.getAvailableForPayment](../../product-rows/crm-item-productrow-get-available-for-payment.md).
3. Add a product item using the method [crm.item.payment.product.add](./crm-item-payment-product-add.md).
4. Check the composition of the items using the method [crm.item.payment.product.list](./crm-item-payment-product-list.md).
5. If necessary, adjust the quantity using the method [crm.item.payment.product.setQuantity](./crm-item-payment-product-set-quantity.md) or remove the item using the method [crm.item.payment.product.delete](./crm-item-payment-product-delete.md).

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
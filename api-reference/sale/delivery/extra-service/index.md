# Additional Services of Delivery Services: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Additional services expand delivery options for specific carriers. For example, you can add options such as Floor Lift or Cargo Insurance.

This is necessary so that the customer can select additional options during the checkout process. Bitrix24 takes the cost of these services into account in the final delivery amount.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [Delivery Services](https://helpdesk.bitrix24.com/open/17297482/)

## Connection of Additional Services with Other Objects

**Delivery Service.** The identifier `DELIVERY_ID` links the service to a specific carrier or delivery method.

**Service Type.** The parameter `TYPE` defines the logic for the customer to select the service:
- `enum` — list of options
- `checkbox` — single service
- `quantity` — quantitative service

**Service Cost.** The method of specifying the price depends on the type. For `checkbox` and `quantity`, use the `PRICE` field. For the `enum` type, specify the price in `ITEMS[].PRICE` for each individual option.

## How to Work with Additional Services

1. Retrieve the delivery service identifier using the method [sale.delivery.getlist](../delivery/sale-delivery-get-list.md).
2. Create a new service using the method [sale.delivery.extra.service.add](./sale-delivery-extra-service-add.md). Specify the service type and link it to the service through the `DELIVERY_ID` parameter.
3. Check the result using the method [sale.delivery.extra.service.get](./sale-delivery-extra-service-get.md). It will return a list of all services for the selected delivery service. Use the `ID` from the response in the next step.
4. If you need to modify the service, call the method [sale.delivery.extra.service.update](./sale-delivery-extra-service-update.md). If you need to delete the service, use the method [sale.delivery.extra.service.delete](./sale-delivery-extra-service-delete.md).

## Overview of Methods {#all-methods}

> Scope: [`sale, delivery`](../../../scopes/permissions.md)
>
> Who can execute the methods: administrator

#| 
|| **Method** | **Description** ||
|| [sale.delivery.extra.service.add](./sale-delivery-extra-service-add.md) | Adds a delivery service ||
|| [sale.delivery.extra.service.update](./sale-delivery-extra-service-update.md) | Updates a delivery service ||
|| [sale.delivery.extra.service.get](./sale-delivery-extra-service-get.md) | Returns a list of services for a specific delivery service ||
|| [sale.delivery.extra.service.delete](./sale-delivery-extra-service-delete.md) | Deletes a delivery service ||
|#
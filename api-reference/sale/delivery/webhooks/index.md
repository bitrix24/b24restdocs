# Webhooks for Delivery Operations

Bitrix24 notifies the delivery service when certain events occur by sending HTTP requests in JSON format to specific URLs that were specified as parameters when creating the delivery service handler in the method [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md). The list of such events includes:

1. Delivery cost calculation — the manager wants to estimate the preliminary delivery cost
2. Creating a delivery order — the manager wants to place a delivery order
3. Cancelling a delivery order — the manager is attempting to cancel a previously placed delivery order

The required response format for requests is JSON.

## Continue Learning

- [{#T}](./calculate.md)
- [{#T}](./create-delivery-request.md)
- [{#T}](./cancel-delivery-request.md)
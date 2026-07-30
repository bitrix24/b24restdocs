# Webhooks for Delivery Operations: Event Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 notifies the delivery service when calculating the delivery total, creating a delivery order, or canceling an order. For each action, Bitrix24 sends a JSON HTTP request to the URL specified in the delivery service handler settings.

Handler URLs are defined in the parameters of the [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md) method. Through these URLs, the external delivery service receives order data and returns the processing result in JSON.

> Quick navigation: [All Events](#all-events)
>
> User documentation: [Delivery Services](https://helpdesk.bitrix24.com/open/17297482/)

## How to Receive Events

1. Create a delivery service handler using the [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md) method
2. Specify the URLs for calculating, creating, and canceling a delivery order in the `SETTINGS` parameter
3. Implement the processing of inbound JSON HTTP requests
4. Return a JSON response in the format described on the relevant event page

## Handler URLs

#|
|| **parameter** | **Event** | **When Bitrix24 sends a request** ||
|| `CALCULATE_URL` | [Calculate shipping cost](./calculate.md) | When the manager calculates the preliminary shipping cost ||
|| `CREATE_DELIVERY_REQUEST_URL` | [Create delivery order](./create-delivery-request.md) | When the manager places a delivery order ||
|| `CANCEL_DELIVERY_REQUEST_URL` | [Cancel delivery order](./cancel-delivery-request.md) | When the manager cancels a previously placed delivery order ||
|#

`CALCULATE_URL` is required when creating a delivery service handler. Specify the URLs for creating and canceling an order if the delivery service supports checking out and canceling orders from Bitrix24.

## Exchange Format

Bitrix24 sends an HTTP request with a JSON body to the handler URL. The request structure depends on the event and is described on the specific event page.

The external handler must return a JSON response:

- A successful response confirms the calculation, creation, or cancellation of the order
- An error response provides a reason that can be displayed to the manager

## Handler Server Availability

Handler URLs must be accessible for inbound requests from Bitrix24. If the delivery service server is unavailable or returns a response in an unexpected JSON format, the delivery action will result in an error.

## Overview of Events {#all-events}

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can receive events: delivery service handler

#|
|| **Event** | **Triggered** ||
|| [Calculate shipping cost](./calculate.md) | During preliminary shipping cost calculation ||
|| [Create delivery order](./create-delivery-request.md) | When placing a delivery order ||
|| [Cancel delivery order](./cancel-delivery-request.md) | When canceling a previously placed delivery order ||
|#

# Webhooks for Delivery Operations: Event Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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

The top level of the request depends on the action:

- for cost calculation — the `SHIPMENT` object
- for order creation — the `SHIPMENTS` array
- for order cancellation — the `DELIVERY_ID` and `REQUEST_ID` identifiers

Abbreviated example of an order creation request:

```json
{
    "SHIPMENTS": [
        {
            "ID": 4063,
            "DELIVERY_SERVICE": {
                "ID": 225
            },
            "ITEMS": []
        }
    ]
}
```

The external handler must return JSON with the `SUCCESS` field. The response fields depend on the result and event:

#|
|| **Field**
`type` | **When to Pass** ||
|| **SUCCESS**
[`string`](../../../data-types.md) | Always: `Y` for successful processing, `N` for an error ||
|| **PRICE**
[`double`](../../../data-types.md) | For successful cost calculation ||
|| **REQUEST_ID**
[`string`](../../../data-types.md) | For successful delivery order creation ||
|| **REASON.TEXT**
[`string`](../../../data-types.md) | On error: the reason text for the manager ||
|#

No additional fields are required for successful order cancellation. Example of a successful order creation response:

```json
{
    "SUCCESS": "Y",
    "REQUEST_ID": "4757aca4931a4f029f49c0db4374d13d"
}
```

On error, provide a description in `REASON.TEXT`:

```json
{
    "SUCCESS": "N",
    "REASON": {
        "TEXT": "Delivery is not available for the specified address"
    }
}
```

The complete set of request and response fields is provided on the event pages.

## Handler Server Availability

Handler URLs must be accessible for inbound requests from Bitrix24. The handler must return HTTP status `200` and a valid JSON object. Responses with other HTTP statuses and data that cannot be parsed as JSON are not processed as a successful delivery service response.

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

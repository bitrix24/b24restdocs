# Cancel Delivery Order CANCEL_DELIVERY_REQUEST_URL

The request is sent to the address specified in `CANCEL_DELIVERY_REQUEST_URL` when creating a delivery handler using the method [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md).

## Request Parameters

#|
|| **Name**
`type` | **Description** ||
|| **DELIVERY_ID**
[`sale_delivery_service.ID`](../../data-types.md#sale_delivery_service) | Identifier of the delivery service.

You can obtain the identifiers of delivery services using the method [sale.delivery.getlist](../delivery/sale-delivery-get-list.md)
 ||
|| **REQUEST_ID**
[`string`](../../../data-types.md) | Identifier of the transport request.

The identifier is assigned by the external system in response to the webhook for creating a delivery order (for more details, see the webhook description [{#T}](./create-delivery-request.md))
 ||
|#

## Example Request

```json
{
    "DELIVERY_ID": 694,
    "REQUEST_ID": "4757aca4931a4f029f49c0db4374d13d"
}
```

## Response Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SUCCESS***
[`string`](../../../data-types.md) | Indicator of the success of the delivery order cancellation. Possible values:

- `Y` — order successfully canceled
- `N` — an error occurred while attempting to cancel the order
 ||
|| **REASON**
[`object`](../../../data-types.md) | Reason for the error. Provided in case of an unsuccessful attempt to cancel the delivery order (detailed description is provided [below](#reason)) ||
|#

### REASON

#|
|| **Name**
`type` | **Description** ||
|| **TEXT***
[`string`](../../../data-types.md) | Description of the error ||
|#

## Example Response with Successful Delivery Order Cancellation

```json
{
    "SUCCESS": "Y"
}
```

## Example Response with Error in Cost Calculation

```json
{
    "SUCCESS": "N",
    "REASON": {
        "TEXT": "The order has been already delivered"
    }
}
```

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calculate.md)
- [{#T}](./create-delivery-request.md)
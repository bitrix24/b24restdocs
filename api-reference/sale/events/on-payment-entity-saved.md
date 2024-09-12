# After Saving Payment OnPaymentEntitySaved

> Scope: [`sale`](../../scopes/permissions.md) 
>
> Who can subscribe: any user

The event `OnPaymentEntitySaved` occurs immediately after a payment is saved.

## What the Handler Receives

Data is sent as a POST request

```
[
    'event' => 'ONPAYMENTENTITYSAVED',
    'eventId' => 1,
    'data' => [
        'FIELDS' => [
            'ID' => 300,
        ],
    ],
    'ts' => 1714649632,
    'auth' => [
        'access_token' => 's6p6eclrvim6da22ft9ch94ekreb52lv',
        'expires_in' => 3600,
        'scope' => 'sale',
        'domain' => 'some-domain.bitrix24.com',
        'server_endpoint' => 'https://oauth.bitrix.info/rest/',
        'status' => 'F',
        'client_endpoint' => 'https://some-domain.bitrix24.com/rest/',
        'member_id' => 'a223c6b3710f85df22e9377d6c4f7553',
        'refresh_token' => '4s386p3q0tr8dy89xvmt96234v3dljg8',
        'application_token' => '51856fefc120afa4b628cc82d3935cce',
    ],
]
```

## Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic code of the event ||
|| **eventId***
[`integer`](../../data-types.md) | Identifier of the event ||
|| **data***
[`object`](../../data-types.md) | Object containing event data ||
|| **ts***
[`integer`](../../data-types.md) | Timestamp of the event sent from the event queue ||
|| **auth***
[`object`](../../data-types.md) | Object with authorization parameters and information about the account where the event occurred ||
|#

### Parameter data

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS***
[`object`](../../data-types.md) | Object with the property `ID`, containing the payment identifier ||
|#

#### Parameter FIELDS

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_order_payment.id`](../data-types.md) | Identifier of the payment. To retrieve all payment fields by identifier, use the method [sale.payment.get](../payment/sale-payment-get.md) ||
|#

### Parameter auth

{% include notitle [Parameter auth](../../../_includes/auth-params-in-events.md) %}
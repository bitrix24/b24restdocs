# At the End of Order Saving OnSaleOrderSaved

> Scope: [`sale`](../../scopes/permissions.md) 
>
> Who can subscribe: any user

The `OnSaleOrderSaved` event occurs at the end of the order saving process, when the order and all related entities have already been saved.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted in the form of a POST request

```
[
    'event' => 'ONSALEORDERSAVED',
    'event_handler_id' => 1,
    'data' => [
        'FIELDS' => [
            'ID' => 300,
            'XML_ID' => '',
            'ACTION' => 'save',
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

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***  
[`string`](../../data-types.md) | Event symbolic code ||
|| **event_handler_id***  
[`integer`](../../data-types.md) | Event handler identifier ||
|| **data***  
[`object`](../../data-types.md) | Object with event data ||
|| **ts***  
[`integer`](../../data-types.md) | Timestamp of the event sent from the event queue ||
|| **auth***  
[`object`](../../data-types.md) | Object with authorization parameters and information about the account where the event occurred ||
|#

### Parameter data

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS***  
[`object`](../../data-types.md) | Object with order properties ||
|#

#### Parameter FIELDS

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***  
[`sale_order.id`](../data-types.md) | Order identifier. To retrieve all order fields by identifier, use the [sale.order.get](../order/sale-order-get.md) method ||
|| **XML_ID***  
[`string`](../data-types.md) | External order identifier ||
|| **ACTION***  
[`string`](../../data-types.md) | Action. For this event, it has a constant value of `save` ||
|#

### Parameter auth

{% include notitle [Parameter auth](../../../_includes/auth-params-in-events.md) %}
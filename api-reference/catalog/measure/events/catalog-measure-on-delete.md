# Event on Deleting a Unit of Measurement CATALOG.MEASURE.ON.DELETE

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event occurs when a unit of measurement is deleted.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```
[
    'event' => 'CATALOG.MEASURE.ON.DELETE',    
    'event_handler_id' => 1,
    'data' => [
        'FIELDS' => [
            'ID' => 1,
        ],
    ],
    'ts' => 1714649632,
    'auth' => [
        'access_token' => 's6p6eclrvim6da22ft9ch94ekreb52lv',
        'expires_in' => 3600,
        'scope' => 'catalog',
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

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic event code ||
|| **event_handler_id***
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data***
[`object`](../../data-types.md) | Object containing event data.

The structure is described [below](#data) ||
|| **ts***
[`integer`](../../data-types.md) | Timestamp of the event sent from the event queue ||
|| **auth***
[`object`](../../data-types.md) | Object with authorization parameters and information about the account where the event occurred ||
|#

### Parameter data {#data}

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS***
[`object`](../../data-types.md) | Object with properties of the unit of measurement.

The structure is described [below](#fields) ||
|#

### Parameter FIELDS {#fields}

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`catalog_measure.id`](../../data-types.md#catalog_measure) | Identifier of the unit of measurement. You can retrieve all fields of the unit of measurement by its identifier using the method [catalog.measure.get](../catalog-measure-get.md) ||
|#

### Parameter auth {#auth}

{% include notitle [Table with Keys in the auth Array](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](./catalog-measure-on-add.md)
- [{#T}](./catalog-measure-on-update.md)
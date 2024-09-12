# Event on User Field Update of Requisite onCrmRequisiteUserFieldUpdate

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event `onCrmRequisiteUserFieldUpdate` is triggered when a user field of a requisite is modified.

## What the handler receives

Data is sent as a POST request {.b24-info}

```php
[
    'event' => 'onCrmRequisiteUserFieldUpdate',
    'data' => [
        'FIELDS' => [
            'ID' => 235,
            'ENTITY_ID' => 'CRM_REQUISITE',
            'FIELD_NAME' => 'NEWTECH_v1_STRING'
        ],
    ],
    'ts' => '1466439714',
    'auth' => [
        'access_token' => 's6p6eclrvim6da22ft9ch94ekreb52lv',
        'expires_in' => '3600',
        'scope' => 'crm',
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

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | Symbolic code of the event. In this case, it is `onCrmRequisiteUserFieldUpdate`||
|| **data***
[`array`](../../../data-types.md) | Array containing the data of the modified user field of the requisite ||
|| **ts***
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../events/index.md) ||
|| **auth***
[`array`](../../../data-types.md) | Authorization parameters and information about the account where the event occurred ||
|#

### Parameter data[]

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **FIELDS***
[`array`](../../../data-types.md) | Array containing the fields of the modified user field of the requisite ||
|#

### Parameter FIELDS[]

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID***
[`integer`](../../../data-types.md) | Identifier of the modified user field of the requisite ||
|| **ENTITY_ID***
[`string`](../../../data-types.md) | Symbolic identifier of the object for which the field was modified ||
|| **FIELD_NAME***
[`string`](../../../data-types.md) | Symbolic code of the modified user field ||
|#

### Parameter auth[]

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue your exploration

- [{#T}](./on-crm-address-register.md)
- [{#T}](./on-crm-address-unregister.md)
- [{#T}](./on-crm-requisite-add.md)
- [{#T}](./on-crm-requisite-update.md)
- [{#T}](./on-crm-requisite-delete.md)
- [{#T}](./on-crm-requisite-user-field-add.md)
- [{#T}](./on-crm-requisite-user-field-delete.md)
- [{#T}](./on-crm-bank-detail-add.md)
- [{#T}](./on-crm-bank-detail-update.md)
- [{#T}](./on-crm-bank-detail-delete.md)
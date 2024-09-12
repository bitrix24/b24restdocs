# Event on Changing the Set of Values for a Custom List Field onCrmRequisiteUserFieldSetEnumValues

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event `onCrmRequisiteUserFieldSetEnumValues` is triggered when the set of values for a custom list field is changed.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```php
[
    'event' => 'onCrmRequisiteUserFieldSetEnumValues',
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

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | Symbolic code of the event. In this case, it is `onCrmRequisiteUserFieldSetEnumValues`||
|| **data***
[`array`](../../../data-types.md) | Array containing data of the custom field of the requisite for which the set of values was changed ||
|| **ts***
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../events/index.md) ||
|| **auth***
[`array`](../../../data-types.md) | Authorization parameters and information about the account where the event occurred ||
|#

### Parameter data[]

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **FIELDS***
[`array`](../../../data-types.md) | Array containing the fields of the custom field of the requisite for which the set of values was changed ||
|#

### Parameter FIELDS[]

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID***
[`integer`](../../../data-types.md) | Identifier of the custom field of the requisite for which the set of values was changed ||
|| **ENTITY_ID***
[`string`](../../../data-types.md) | Symbolic identifier of the object to which the custom field belongs ||
|| **FIELD_NAME***
[`string`](../../../data-types.md) | Symbolic code of the custom field for which the set of values was changed ||
|#

### Parameter auth[]

{% include notitle [Table of Keys in the auth Array](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](./on-crm-address-register.md)
- [{#T}](./on-crm-address-unregister.md)
- [{#T}](./on-crm-requisite-add.md)
- [{#T}](./on-crm-requisite-update.md)
- [{#T}](./on-crm-requisite-delete.md)
- [{#T}](./on-crm-requisite-user-field-add.md)
- [{#T}](./on-crm-requisite-user-field-update.md)
- [{#T}](./on-crm-requisite-user-field-delete.md)
- [{#T}](./on-crm-bank-detail-add.md)
- [{#T}](./on-crm-bank-detail-update.md)
- [{#T}](./on-crm-bank-detail-delete.md)
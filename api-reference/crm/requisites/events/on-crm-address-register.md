# Event onCrmAddressRegister

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The `onCrmAddressRegister` event is triggered when an address is registered.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```php
[
    'event' => 'onCrmAddressRegister',
    'data' => [
        'FIELDS' => [
            'TYPE_ID' => 1,
            'ENTITY_TYPE_ID' => 8,
            'ENTITY_ID' => 1,
            'ANCHOR_ID' => 17192,
            'ANCHOR_TYPE_ID' => 3,
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
[`string`](../../../data-types.md) | Symbolic event code. In this case, it is `onCrmAddressRegister`||
|| **data***
[`array`](../../../data-types.md) | Array with the registered address data ||
|| **ts***
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth***
[`array`](../../../data-types.md) | Authorization parameters and information about the account where the event occurred ||
|#

### Parameter data[]

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **FIELDS***
[`array`](../../../data-types.md) | Array with the fields of the registered address ||
|#

### Parameter FIELDS[]

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the address type. Enumeration element "Address Type".

The enumeration elements "Address Type" are returned by the method [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md)
||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the parent object type.

Object type identifiers are returned by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

Addresses can only be linked to Requisites (and requisites are linked to companies or contacts) or Leads. For backward compatibility, the ability to link Addresses to Contacts or Companies is retained. However, this link is only possible on some older accounts where the old address handling mode was specifically enabled by support 
||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Identifier of the parent object ||
|| **ANCHOR_ID***
[`integer`](../../../data-types.md) | Identifier of the main parent object.

This field is for internal use. The value is automatically filled when adding an address.

This field contains the identifier of the parent object of the requisite (company or contact) if the address is linked to the requisite. If the address is linked to a lead, then this value will be the lead identifier
||
|| **ANCHOR_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the type of the main parent object.

This field is for internal use. The value is automatically filled when adding an address.

Object type identifiers are returned by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

This field contains the identifier of the type of the parent object of the requisite (company or contact) if the address is linked to the requisite. If the address is linked to a lead, then this value will be the lead type identifier
||
|#

### Parameter auth[]

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue your study

- [{#T}](./on-crm-address-unregister.md)
- [{#T}](./on-crm-requisite-add.md)
- [{#T}](./on-crm-requisite-update.md)
- [{#T}](./on-crm-requisite-delete.md)
- [{#T}](./on-crm-requisite-user-field-add.md)
- [{#T}](./on-crm-requisite-user-field-set-enum-values.md)
- [{#T}](./on-crm-requisite-user-field-update.md)
- [{#T}](./on-crm-requisite-user-field-delete.md)
- [{#T}](./on-crm-bank-detail-add.md)
- [{#T}](./on-crm-bank-detail-update.md)
- [{#T}](./on-crm-bank-detail-delete.md)
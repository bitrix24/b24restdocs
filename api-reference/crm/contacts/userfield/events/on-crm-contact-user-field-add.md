# Event for Adding a Custom Field onCrmContactUserFieldAdd

> Scope: [`crm`](../../../../scopes/permissions.md)
> 
> Who can subscribe: any user

The event triggers when a custom field is added to a contact.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONCRMCONTACTUSERFIELDADD",
    "event_handler_id": "14",
    "data": {
        "FIELDS": {
            "ID": "553",
            "ENTITY_ID": "CRM_CONTACT",
            "FIELD_NAME": "UF_CRM_1724770216"
        }
    },
    "ts": "1724770216",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "crm",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMCONTACTUSERFIELDADD`||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | Object containing information about the created custom field.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | Object containing information about the fields of the created custom field.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../../../data-types.md) | Date and time the event was sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`](../../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the created custom field ||
|| **ENTITY_ID**
[`userFieldEntityId`](../../../data-types.md#object_type) | Type of CRM object to which the custom field is attached.

In this case — `CRM_CONTACT` ||
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Code of the created custom field ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./on-crm-contact-user-field-update.md)
- [{#T}](./on-crm-contact-user-field-set-enum-values.md)
- [{#T}](./on-crm-contact-user-field-delete.md)
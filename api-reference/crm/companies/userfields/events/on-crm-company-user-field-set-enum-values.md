# When setting list values for the custom field onCrmCompanyUserFieldSetEnumValues

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONCRMCOMPANYUSERFIELDSETENUMVALUES` will trigger when setting list values for a company's custom field.

## What the handler receives

Data is sent as a POST request {.b24-info}

```json
{
    "event": "ONCRMCOMPANYUSERFIELDSETENUMVALUES",
    "event_handler_id": "745",
    "data": {
        "FIELDS": {
            "ID": "6979",
            "ENTITY_ID": "CRM_COMPANY",
            "FIELD_NAME": "UF_CRM_1743165530"
        }
    },
    "ts": "1743165530",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "crm",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "L",
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
[`string`](../../../../data-types.md) | Symbolic event code.

In this case — `ONCRMCOMPANYUSERFIELDSETENUMVALUES` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Event handler identifier ||
|| **data**
[`object`](../../../../data-types.md) | Object containing information about the custom field for which list values are set.

Contains the key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | Object containing information about the fields of the custom field.

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
[`integer`](../../../../data-types.md) | Identifier of the custom field ||
|| **ENTITY_ID**
[`string`](../../../../data-types.md) | Identifier of the object to which the custom field belongs. In this case — `CRM_COMPANY` ||
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Name of the custom field ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue exploring

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./on-crm-company-user-field-add.md)
- [{#T}](./on-crm-company-user-field-update.md)
- [{#T}](./on-crm-company-user-field-delete.md)
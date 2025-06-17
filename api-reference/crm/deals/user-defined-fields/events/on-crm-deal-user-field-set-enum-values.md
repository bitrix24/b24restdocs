# Event on changing the set of values for a custom list-type field onCrmDealUserFieldSetEnumValues

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmDealUserFieldSetEnumValues` will trigger when the set of values for a custom list-type field is changed.

## What the handler receives

Data is sent as a POST request {.b24-info}

```json
{
    "event": "ONCRMDEALUSERFIELDSETENUMVALUES",
    "event_handler_id": "665",
    "data": {
        "FIELDS": {
            "ID": "6947",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1736930561"
        }
    },
    "ts": "1736944167",
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
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMDEALUSERFIELDSETENUMVALUES`||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing information about the updated list-type field.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../data-types.md) | Object containing information about the fields of the updated list-type field.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the custom list-type field ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Symbolic identifier of the object for which the list-type field was updated. In this case — `CRM_DEAL` ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Name of the updated custom list-type field ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue exploring

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./on-crm-deal-user-field-add.md)
- [{#T}](./on-crm-deal-user-field-delete.md)
- [{#T}](./on-crm-deal-user-field-update.md)
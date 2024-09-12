# Event for Deleting Custom CRM Type onCrmTypeDelete

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event is triggered when a [custom CRM type](../../user-defined-object-types/index.md) is deleted.

## What the Handler Receives

Data is sent as a POST request {.b24-info}

```json
{
    "event": "ONCRMTYPEDELETE",
    "event_handler_id": "9",
    "data": {
        "FIELDS": {
            "ID": "45"
        }
    },
    "ts": "1723546073",
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
[`string`][1] | Symbolic code of the event.

In this case — `ONCRMTYPEDELETE`||
|| **event_handler_id**
[`integer`][1] | Identifier of the event handler ||
|| **data**
[`object`][1] | Object containing information about the deleted custom CRM type.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`][1] | Object containing information about the fields of the deleted custom CRM type.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`][1] | Date and time the event was sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`][1] | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### FIELDS Parameter {#fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`][1] | Identifier of the deleted custom CRM type (primary key, not the type identifier) ||
|#

### auth Parameter {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue Exploring

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](index.md)
- [{#T}](on-crm-type-add.md)
- [{#T}](on-crm-type-update.md)

[1]: ../../../../data-types.md
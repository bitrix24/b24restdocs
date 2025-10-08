# Event when creating a document onCrmDocumentGeneratorDocumentAdd

> Scope: [`documentgenerator, crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONCRMDOCUMENTGENERATORDOCUMENTADD` will trigger when a new document is created.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the handler receives

Data is sent as a POST request {.b24-info}

```json
{
    "event": "ONCRMDOCUMENTGENERATORDOCUMENTADD",
    "event_handler_id": "821",
    "data": {
        "FIELDS": {
            "ID": "1869",
            "ENTITY_TYPE_ID": "1",
            "ENTITY_ID": "1000993"
        }
    },
    "ts": "1749042094",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "crm, documentgenerator",
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
[`string`](../../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMDOCUMENTGENERATORDOCUMENTADD` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | Object containing information about the created document.

Contains the key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | Object containing information about the fields of the created document.

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
[`integer`](../../../../data-types.md) | Identifier of the created document ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../../data-types.md) | Identifier of the [object type](../../../../crm/data-types.md#object_type) to which the document belongs, for example `1` — lead ||
|| **ENTITY_ID**
[`integer`](../../../../data-types.md) | Identifier of the entity to which the document is linked ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue exploring

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./on-crm-document-generator-document-update.md)
- [{#T}](./on-crm-document-generator-document-delete.md)
# When Adding a Custom Field of a Smart Process, New Invoice, or Document onCrmTypeUserFieldAdd

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONCRMTYPEUSERFIELDADD` will trigger when a custom field is added for a smart process, a new invoice, or a document for signing. The event arrives both when the field is added in the interface and when the [userfieldconfig.add](../userfieldconfig-add.md) method is called.

If a field of the list type `enumeration` is added together with a set of values, the handler receives two events: `onCrmTypeUserFieldAdd` and [onCrmTypeUserFieldSetEnumValues](./on-crm-type-user-field-set-enum-values.md). All cases are covered in the [Which Events Arrive Together](./index.md#pairs) section.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONCRMTYPEUSERFIELDADD",
    "event_handler_id": "713",
    "data": {
        "FIELDS": {
            "ID": "6977",
            "ENTITY_ID": "CRM_13",
            "FIELD_NAME": "UF_CRM_13_1742999523"
        }
    },
    "ts": "1742999523",
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
|| **parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMTYPEUSERFIELDADD` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | An object containing information about the added custom field.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | An object containing the identifiers of the added custom field.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../../../data-types.md) | Date and time of the event sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`](../../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the added custom field.

Pass it together with `moduleId = crm` to the [userfieldconfig.get](../userfieldconfig-get.md) method to retrieve the field type and its settings ||
|| **ENTITY_ID**
[`string`](../../../../data-types.md) | Identifier of the object the custom field belongs to: `CRM_{id}` for a smart process, `CRM_SMART_INVOICE` for a new invoice, `CRM_SMART_DOCUMENT` for a document for signing.

The `CRM_{id}` format uses the `id` key from the result of the [crm.type.list](../../user-defined-object-types/crm-type-list.md) method, not `entityTypeId`.

The values are described in the [Which Objects Trigger the Events](./index.md#objects) section ||
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Symbolic code of the added custom field, for example `UF_CRM_13_1742999523` ||
|#

The event does not pass the field type, its settings, or the list of values. Retrieve them with the [userfieldconfig.get](../userfieldconfig-get.md) method. This method requires the `userfieldconfig` scope in addition to the `crm` scope the application subscribes to the event with.

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-crm-type-user-field-update.md)
- [{#T}](./on-crm-type-user-field-delete.md)
- [{#T}](./on-crm-type-user-field-set-enum-values.md)

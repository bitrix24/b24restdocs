# Event on User Field Change onCrmDealUserFieldUpdate

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onCrmDealUserFieldUpdate` event triggers when a user field is changed.

The event refers to the configuration of a custom field rather than the value of this field in a specific deal.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONCRMDEALUSERFIELDUPDATE",
    "event_handler_id": "661",
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
|| **parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMDEALUSERFIELDUPDATE` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | An object containing information about the updated custom field.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | An object containing custom field properties.

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
[`integer`](../../../../data-types.md) | Identifier of the custom field ||
|| **ENTITY_ID**
[`string`](../../../../data-types.md) | Symbolic identifier of the object for which the field was updated. In this case — `CRM_DEAL` ||
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Name of the updated custom field ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./on-crm-deal-user-field-add.md)
- [{#T}](./on-crm-deal-user-field-delete.md)
- [{#T}](./on-crm-deal-user-field-set-enum-values.md)

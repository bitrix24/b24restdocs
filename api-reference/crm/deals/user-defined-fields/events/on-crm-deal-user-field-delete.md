# Event onCrmDealUserFieldDelete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onCrmDealUserFieldDelete` event will trigger when a custom field is deleted manually or via the [crm.deal.userfield.delete](../crm-deal-userfield-delete.md) method.

The event refers to the configuration of a custom field rather than the value of this field in a specific deal.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONCRMDEALUSERFIELDDELETE",
    "event_handler_id": "663",
    "data": {
        "FIELDS": {
            "ID": "5227",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1592601331"
        }
    },
    "ts": "1736943852",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "crm",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "L",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

#|
|| **parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMDEALUSERFIELDDELETE` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | Object containing information about the deleted custom field.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | An object with the ID and code of the custom field.

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
[`string`](../../../../data-types.md) | Symbolic code of the object the field belongs to. In this case — `CRM_DEAL` ||
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Custom field code with the `UF_CRM_` prefix ||
|#

The settings of the deleted field are not included in the event and cannot be retrieved after deletion: the [crm.deal.userfield.get](../crm-deal-userfield-get.md) method with the `ID` from the event returns the `ERROR_NOT_FOUND` error.

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-crm-deal-user-field-add.md)
- [{#T}](./on-crm-deal-user-field-update.md)
- [{#T}](./on-crm-deal-user-field-set-enum-values.md)

# Event on Deal Update onCrmDealUpdate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user. Administrator permissions are required for offline events

The event `ONCRMDEALUPDATE` will trigger when a deal is updated.

Changing the pipeline with [crm.item.update](../../universal/crm-item-update.md) also triggers `ONCRMDEALUPDATE`. Moving a deal to another pipeline from the deal form or with the "Change pipeline" automation rule does not trigger this event. In these cases, [onCrmDealMoveToCategory](./on-crm-deal-move-to-category.md) is triggered.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md).

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONCRMDEALUPDATE",
    "event_handler_id": "201",
    "data": {
        "FIELDS": {
            "ID": "759"
        }
    },
    "ts": "1736405807",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "crm",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMDEALUPDATE` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing information about the updated deal.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../data-types.md) | An object containing the identifier of the updated deal.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object containing authorization parameters and information about the Bitrix24 account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the updated deal. In the POST request example, it is passed as a string ||
|#

The event passes only the deal `ID` and does not indicate which fields changed. Current field values can be retrieved using [crm.deal.get](../crm-deal-get.md).

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-crm-deal-add.md)
- [{#T}](./on-crm-deal-delete.md)
- [{#T}](./on-crm-deal-move-to-category.md)
- [{#T}](../crm-deal-get.md)

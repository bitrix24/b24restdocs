# Event on Changing the Deal's Sales Funnel onCrmDealMoveToCategory

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user. Administrator permissions are required for offline events

The event `ONCRMDEALMOVETOCATEGORY` will trigger when a deal is moved to another pipeline from the deal form or with the "Change pipeline" automation rule. Changing the pipeline with [crm.item.update](../../universal/crm-item-update.md) does not trigger this event. In this case, [onCrmDealUpdate](./on-crm-deal-update.md) is triggered.

You can only subscribe to the event from the [application](../../../../settings/app-installation/index.md).

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md).

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONCRMDEALMOVETOCATEGORY",
    "event_handler_id": "655",
    "data": {
        "FIELDS": {
            "ID": "6675",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW"
        }
    },
    "ts": "1736424182",
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
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMDEALMOVETOCATEGORY` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing information about the modified deal.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../data-types.md) | An object containing the deal identifier, the new pipeline, and the new stage.

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
[`integer`](../../../data-types.md) | Identifier of the modified deal. In the POST request example, it is passed as a string ||
|| **CATEGORY_ID**
[`integer`](../../../data-types.md) | Identifier of the new Sales Funnel. In the POST request example, it is passed as a string ||
|| **STAGE_ID**
[`string`](../../../data-types.md) | Identifier of the new deal stage ||
|#

The event does not pass other deal data. It can be retrieved using [crm.deal.get](../crm-deal-get.md).

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-crm-deal-add.md)
- [{#T}](./on-crm-deal-update.md)
- [{#T}](./on-crm-deal-delete.md)
- [{#T}](../crm-deal-get.md)

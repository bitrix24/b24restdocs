# Event on Updating a Comment in the CRM Timeline onCrmTimelineCommentUpdate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onCrmTimelineCommentUpdate` event is triggered when a comment is updated in the CRM timeline.

When a comment with multiple bindings is updated, the event contains the identifier of the new comment. For details, see the [crm.timeline.comment.update](../crm-timeline-comment-update.md) method description.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONCRMTIMELINECOMMENTUPDATE",
    "event_handler_id": "185",
    "data": {
        "FIELDS": {
            "ID": 999
        }
    },
    "ts": "1466439714",
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

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **parameter**
`type` | **Description** ||
|| **event***
[`string`](../../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMTIMELINECOMMENTUPDATE` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data***
[`object`](../../../../data-types.md) | An object containing data about the updated comment.

Contains a single key `FIELDS` ||
|| **data.FIELDS***
[`object`](../../../../data-types.md) | An object containing the identifier of the updated comment.

The structure is described [below](#fields) ||
|| **ts***
[`timestamp`](../../../../data-types.md) | Date and time of the event sent from the [event queue](../../../../events/index.md) ||
|| **auth***
[`object`](../../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **parameter**
`type` | **Description** ||
|| **ID***
[`integer`](../../../../data-types.md) | Identifier of the updated comment ||
|#

To retrieve the comment data, pass `data.FIELDS.ID` in the `id` parameter of the [crm.timeline.comment.get](../crm-timeline-comment-get.md) method.

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-Crm-Timeline-Comment-Add.md)
- [{#T}](./on-Crm-Timeline-Comment-Delete.md)
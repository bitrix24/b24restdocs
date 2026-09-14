# Event for Deleting a Message from the News Feed OnLiveFeedPostDelete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONLIVEFEEDPOSTDELETE` event is triggered when a message is deleted from the Feed.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONLIVEFEEDPOSTDELETE",
    "event_handler_id": "729",
    "data": {
        "FIELDS": {
            "POST_ID": "209"
        }
    },
    "ts": "1742999814",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "log",
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
[`string`](../../data-types.md) | Symbolic code of the event.

In this case — `ONLIVEFEEDPOSTDELETE` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../data-types.md) | Object containing information about the message deletion from the News Feed.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../data-types.md) | Object containing information about the message deleted from the News Feed.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../data-types.md) | Date and time of the event sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`](../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **parameter**
`type` | **Description** ||
|| **POST_ID**
[`integer`](../../data-types.md) | Identifier of the message deleted from the News Feed ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./on-live-feed-post-add.md)
- [{#T}](./on-live-feed-post-update.md)

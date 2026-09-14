# OnCalendarSectionAdd Event When Adding a Calendar Section or Resource

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONCALENDARSECTIONADD` event is triggered when a calendar section or resource is added.

{% note info " " %}

Technically, a resource is a calendar section. Each resource is placed in a special type of calendar, and a separate section is created for it. Events `OnCalendarSection*` are triggered when resources are added, updated, or deleted.

{% endnote %}

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

Example: event for creating a calendar section object with `id = 202`.

```json
{
    "event": "ONCALENDARSECTIONADD",
    "event_handler_id": "6",
    "data": {
        "id": "202"
    },
    "ts":"1734608536",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "calendar",
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
|| **Name**
`type` | **Description** ||
|| **event**
[`string`](../../data-types.md) | Symbolic code of the event.

In this case — `ONCALENDARSECTIONADD` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../data-types.md) | Object containing information about the added calendar section object.

Contains a single key — `id` ||
|| **data.id**
[`string`](../../data-types.md) | Identifier of the calendar section object ||
|| **ts**
[`timestamp`](../../data-types.md) | Date and time of the event sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`](../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-calendar-section-update.md)
- [{#T}](./on-calendar-section-delete.md)

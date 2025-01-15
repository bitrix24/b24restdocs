# OnCalendarSectionAdd Event When Adding a Calendar Section or Resource

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can subscribe: any user

This event is triggered when a calendar section or resource is added.

{% note info " " %}

Technically, a resource is a calendar section. Each resource is placed in a special type of calendars, and a separate section is created for it. Events `OnCalendarSection*` are triggered when resources are added, updated, or deleted.

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
[`string`][1] | Symbolic event code.

In this case — `ONCALENDARSECTIONADD`||
|| **event_handler_id**
[`integer`][1] | Identifier of the event handler ||
|| **data**
[`object`][1] | Object containing information about the added calendar section object.

Contains a single key — `id` ||
|| **data.id**
[`string`][1] | Identifier of the calendar section object ||
|| **ts**
[`timestamp`][1] | Date and time the event was sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`][1] | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### auth Parameter {#auth}

{% include notitle [Table with keys in the auth array](../../../_includes/auth-params-in-events.md) %}

## Continue Learning 

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-calendar-section-update.md)
- [{#T}](./on-calendar-section-delete.md)

[1]: ../../data-types.md
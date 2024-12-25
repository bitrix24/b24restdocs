# OnCalendarSectionAdd Event

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnCalendarSectionAdd` event is triggered when a calendar section is added ([CALENDAR_SECTION_ID](https://dev.quickbooks.com/user_help/components/content/calendar/calendar_events_list.php)). It will also be invoked when a resource is added.

## What the handler receives

Data is transmitted in the form of a POST request {.b24-info}

Event for creating a calendar section object `id = 202`:

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

{% note info %}

Technically, a resource is equivalent to a section. Since the created resources are placed in a special type of calendars and a section is created for each resource, these events will be triggered when resources are added or removed.

{% endnote %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`][1] | Symbolic code of the event.

In this case — `ONCALENDARSECTIONADD`||
|| **event_handler_id**
[`integer`][1] | Identifier of the event handler ||
|| **data**
[`object`][1] | Object containing information about the added calendar section object.

Contains a single key `id` ||
|| **data.id**
[`string`][1] | Identifier of the calendar section object. ||

|| **ts**
[`timestamp`][1] | Date and time of the event sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`][1] | Object containing authorization parameters and data about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../_includes/auth-params-in-events.md) %}

[1]: ../../data-types.md
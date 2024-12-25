# OnCalendarSectionDelete Event

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnCalendarSectionDelete` event is triggered when a section/resource is deleted. The following data is passed to the handler:

## What the handler receives

Data is sent as a POST request {.b24-info}

Event for deleting a calendar section object `id = 202`:

```json
{
  "event": "ONCALENDARSECTIONDELETE",
  "event_handler_id": "8",
  "data": {
    "id": "202"
  },
  "ts": "1734608547",
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
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`][1] | Symbolic code of the event.

In this case — `ONCALENDARSECTIONDELETE`||
|| **event_handler_id**
[`integer`][1] | Identifier of the event handler ||
|| **data**
[`object`][1] | Object containing information about the deleted calendar section object.

Contains a single key `id` ||
|| **data.id**
[`string`][1] | Identifier of the calendar section object. ||

|| **ts**
[`timestamp`][1] | Date and time the event was sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`][1] | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../_includes/auth-params-in-events.md) %}

[1]: ../../data-types.md
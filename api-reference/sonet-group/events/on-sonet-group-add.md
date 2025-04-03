# Event for Adding a New Workgroup onSonetGroupAdd

> Scope: `sonet`
> 
> Who can subscribe: any user

The `onSonetGroupAdd` event is triggered when a new workgroup/project is added. This allows a third-party application to respond to the creation of new groups and perform necessary actions—such as data synchronization or sending notifications.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONSONETGROUPADD",
    "event_handler_id": "655",
    "data": {
        "FIELDS": {
            "ID": "6675"
        }
    },
    "ts": "1736424182",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "sonet",
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
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../data-types.md) | Symbolic code of the event.

In this case—`ONSONETGROUPADD`||
|| **event_handler_id**
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../data-types.md) | Object containing information about the addition of a new workgroup.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../data-types.md) | Object containing information about the fields of the new workgroup.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../data-types.md) | Date and time of the event sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`](../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {% #fields %}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID** 
[`integer`](../../data-types.md) | Identifier of the new workgroup ||
|#

### Parameter auth

{% include notitle [Table with keys in the auth array](../../../_includes/auth-params-in-events.md) %}

## Continue Exploring
- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
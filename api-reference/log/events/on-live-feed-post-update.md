# Event for Message Update in the News Feed OnLiveFeedPostUpdate

> Scope: [`log`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnLiveFeedPostUpdate` event is triggered after a message in the News Feed is modified. This allows a third-party application to perform necessary actions when messages are changed, such as sending notifications to discussion participants.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONLIVEFEEDPOSTUPDATE",
    "event_handler_id": "731",
    "data": {
        "FIELDS": {
            "POST_ID": "205"
        }
    },
    "ts": "1743000379",
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
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../data-types.md) | Symbolic code of the event.

In this case — `ONLIVEFEEDPOSTUPDATE`||
|| **event_handler_id**
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../data-types.md) | Object containing information about the message change in the News Feed.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../data-types.md) | Object containing information about the modified message in the News Feed.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../data-types.md) | Date and time the event was sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`](../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### FIELDS Parameter {% #fields %}

#|
|| **Parameter**
`type` | **Description** ||
|| **POST_ID** 
[`integer`](../../data-types.md) | Identifier of the modified message in the News Feed ||
|#

### auth Parameter

{% include notitle [Table with keys in the auth array](../../../_includes/auth-params-in-events.md) %}

## Continue Exploring
- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
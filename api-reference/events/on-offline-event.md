# Event onOfflineEvent Change

> Who can subscribe: any user

The `onOfflineEvent` notifies about the occurrence of new offline events at certain intervals.

The application can subscribe to two types of events.

- Regular: the event triggers an external URL and executes an action defined by that address.
- Offline: instead of calling an external URL, events are locally saved on the account, from where they can later be retrieved using the [event.offline.*](./index.md#all-methods) methods.

For the `onOfflineEvent`, the necessity of sending a notification is determined based on the local saving, and then it is sent as a regular event to the external URL.

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```
{
  "event": "ONOFFLINEEVENT",
  "data": {
    "type": "add",
    "event": "onTaskAdd",
    "handler": "https://example.com/handler.php", 
    "minTimeout": 5
  },
  "ts": "1466439714",
  "auth": {
    "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
    "expires_in": "3600",
    "scope": "task",
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

## Request Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../data-types.md) | Symbolic event code — `ONOFFLINEEVENT` ||
|| **data***
[`array`](../data-types.md) | Data about the event in the queue.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../data-types.md) | Date and time of event sending ||
|| **auth***
[`array`](../data-types.md) | Authorization and account data.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../data-types.md) | Type of change: `add`, `remove`, `update` ||
|| **event***
[`string`](../data-types.md) | Event name. For example, `onTaskAdd` ||
|| **handler***
[`string`](../data-types.md) | Event handler URL ||
|| **minTimeout**
[`integer`](../data-types.md) | Minimum delay before sending the event in seconds. Used for event grouping. Default is 1 sec. If the parameter value:
- is `0`, regardless of the number of events added to the offline queue, only one event will be sent to the handler's address within a single hit
- is greater than `0`, upon the first trigger, it sends one event. Then a pause is made for at least the timeout duration before sending the next event
  
The `minTimeout` field appears only if the event was added to the queue with a delay ||
|#

### Parameter auth {#auth}

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **access_token***
[`string`](../data-types.md) | Token for accessing the API ||
|| **expires_in***
[`integer`](../data-types.md) | Time in seconds until the token expires ||
|| **scope***
[`string`](../data-types.md) | [Scope](../scopes/permissions.md) within which the event occurred ||
|| **domain***
[`string`](../data-types.md) | Address of Bitrix24 where the event occurred ||
|| **server_endpoint***
[`string`](../data-types.md) | Address of the Bitrix24 authorization server needed for refreshing OAuth 2.0 tokens ||
|| **status***
[`string`](../data-types.md) | Status of the application that subscribed to this event:

- `L` — [local](../../local-integrations/local-apps.md) application
- `F` — [free mass-market](../../market/index.md) application

||
|| **client_endpoint***
[`string`](../data-types.md) | General path for API method calls for Bitrix24 where the event occurred ||
|| **member_id***
[`string`](../data-types.md) | Identifier of Bitrix24 where the event occurred ||
|| **refresh_token***
[`string`](../data-types.md) | Token for renewing authorization [OAuth 2.0](../oauth/index.md) ||
|| **application_token***
[`string`](../data-types.md) | Token for secure event processing ||
|#

offline_event — the application is not always in a position to receive events. It may be hidden behind firewalls, reside in an internal network, and so on. In this case, the offline event mechanism is used, where the application subscribes to events but does not specify a handler URL.

## Continue Learning

- [{#T}](./events.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
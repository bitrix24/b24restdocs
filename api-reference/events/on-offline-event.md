# onOfflineEvent Queue Change Event

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Who can subscribe: any user

The `onOfflineEvent` event notifies about the occurrence of new offline events at certain intervals.

The application can subscribe to two types of events.

- Regular: the event triggers an external URL and executes an action defined by that address.
- Offline: instead of calling an external URL, events are locally stored on the account, from where they can later be retrieved using the [event.offline.*](./index.md#all-methods) methods.

For the `onOfflineEvent`, the necessity of sending a notification is determined based on the local storage, and then it is sent as a regular event to the external URL.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

The event notifies about new entries in the offline event queue. The data of modified objects is not transmitted — retrieve them using the [event.offline.get](./event-offline-get.md) or [event.offline.list](./event-offline-list.md) methods.

Data is transmitted as a POST request {.b24-info}

```
{
  "event": "ONOFFLINEEVENT",
  "data": [],
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

{% include [Note on Required Parameters](../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **event*** 
[`string`](../data-types.md) | Symbolic event code — `ONOFFLINEEVENT` ||
|| **data*** 
[`array`](../data-types.md) | Arrives empty. More details [below](#data) ||
|| **ts*** 
[`timestamp`](../data-types.md) | Date and time of event sending ||
|| **auth*** 
[`array`](../data-types.md) | Authorization and account data.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

The parameter arrives empty. The event does not transmit data about specific changes but only signals the occurrence of new entries in the queue. To retrieve events, call [event.offline.get](./event-offline-get.md) or [event.offline.list](./event-offline-list.md).

### Parameter auth {#auth}

{% include [Note on Required Parameters](../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **access_token*** 
[`string`](../data-types.md) | Token for API access ||
|| **expires_in*** 
[`integer`](../data-types.md) | Time in seconds until the token expires ||
|| **scope*** 
[`string`](../data-types.md) | [Scope](../scopes/permissions.md) within which the event occurred ||
|| **domain*** 
[`string`](../data-types.md) | Bitrix24 address where the event occurred ||
|| **server_endpoint*** 
[`string`](../data-types.md) | Bitrix24 authorization server address needed for refreshing OAuth 2.0 tokens ||
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
[`string`](../data-types.md) | Token for extending authorization [OAuth 2.0](../../settings/oauth/index.md) ||
|| **application_token*** 
[`string`](../data-types.md) | Token for secure event processing ||
|#

offline_event — the application is not always able to receive events. It may be hidden behind firewalls, reside in an internal network, and so on. In this case, the offline event mechanism is used, where the application subscribes to events but does not specify a handler URL.

## Notification Frequency {#min-timeout}

The interval between notifications is set by the `minTimeout` parameter in the `options` object when subscribing using the [event.bind](./event-bind.md) method. The value is specified in seconds, defaulting to 1. Parameter values:
- `0` — within a single call to Bitrix24, only one notification is sent to the handler address, regardless of the number of events added to the queue
- greater than `0` — upon the first trigger, one notification is sent, and the next will not be sent until the specified number of seconds has passed

## Continue Exploring

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
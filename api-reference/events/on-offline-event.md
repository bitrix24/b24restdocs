# Event on New Entries in the Offline Event Queue onOfflineEvent

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONOFFLINEEVENT` event is triggered when new entries appear in the [offline event](./offline-events.md) queue.

The event replaces polling the queue on a timer: the application receives a signal and retrieves the accumulated entries using the [event.offline.get](./event-offline-get.md) or [event.offline.list](./event-offline-list.md) methods.

Subscribe to `ONOFFLINEEVENT` only as a regular event — with the handler URL in the `handler` parameter of the [event.bind](./event-bind.md) method. Subscribing with `event_type = offline` returns the `ERROR_ARGUMENT` error with the text `Offline event cannot be registered for this event`: the queue notification itself is not placed into the queue.

Any user can subscribe, but only from an application: `event.bind` works with OAuth 2.0 authorization only and rejects a webhook call. Entries can be retrieved from the queue using the `event.offline.*` methods only by an administrator and, again, only in the application context. Read more in the article [{#T}](./offline-events.md).

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONOFFLINEEVENT",
    "event_handler_id": "185",
    "data": [],
    "ts": "1466439714",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "crm,task",
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
|| **event_handler_id*** 
[`integer`](../data-types.md) | Identifier of the event handler ||
|| **data*** 
[`array`](../data-types.md) | Arrives empty.

More details [below](#data) ||
|| **ts*** 
[`timestamp`](../data-types.md) | Date and time the notification was sent from the common [event queue](./index.md) ||
|| **auth*** 
[`object`](../data-types.md) | Object containing authorization parameters and data about the Bitrix24 account where the event occurred.

The `scope` value is the list of permissions granted to the application. The event is basic and does not require a separate scope.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

The event does not transmit data about specific changes but only reports that new entries have appeared in the queue. It carries no payload, so `data` arrives empty — do not build the handler around parsing this parameter.

To retrieve the events themselves, call [event.offline.get](./event-offline-get.md) or [event.offline.list](./event-offline-list.md). The structure of a queue entry is described on the pages of these methods.

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../_includes/auth-params-in-events.md) %}

## Notification Frequency {#min-timeout}

Bitrix24 does not send a separate notification for every queue entry. The minimum interval between notifications is set by the `minTimeout` parameter in the `options` object when subscribing with the [event.bind](./event-bind.md) method.

This is the only `options` parameter that `ONOFFLINEEVENT` supports.

#|
|| **Name**
`type` | **Description** ||
|| **minTimeout**
[`integer`](../data-types.md) | Minimum interval between notifications, in seconds. Defaults to 1.

`0` — the handler receives one notification per request to Bitrix24, no matter how many entries that request added to the queue.

Greater than `0` — the first trigger sends a notification immediately, the next one is sent no earlier than after the specified number of seconds ||
|#

Body of the `event.bind` request for a subscription with a 60-second interval:

```json
{
    "event": "ONOFFLINEEVENT",
    "handler": "https://example.com/handler.php",
    "options": {
        "minTimeout": 60
    }
}
```

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./safe-event-handlers.md)

# Security in Handlers

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In event handlers for applications and outgoing webhooks, verify that the request was sent by Bitrix24 rather than a third-party service. For this check, Bitrix24 passes `auth.application_token` to the handler.

For an application, the parameter is first passed to the [`ONAPPINSTALL`](../common/events/on-app-install.md) event handler together with the authorization data of the user who installed the application. The `ONAPPINSTALL` event handler can verify the received `access_token` and retain `application_token`. Other event handlers must then compare the incoming `auth.application_token` with the retained value.

If the application receives the [`ONAPPUPDATE`](../common/events/on-app-update.md) event, update the retained `application_token`. After a new application version is installed, Bitrix24 passes a new token in the event.

It is especially important to verify the token in the [`ONAPPUNINSTALL`](../common/events/on-app-uninstall.md) event handler, because no authorization data is passed to it: the application has already been removed from Bitrix24. For `ONAPPUNINSTALL`, comparing `application_token` with the retained value becomes the only way to make sure that the event handler was called by Bitrix24.

For an outgoing webhook, the token is created in the Bitrix24 interface after the webhook is saved. Retain the value of the *Application token* field in the outgoing webhook form and compare it with `auth.application_token` in incoming requests.

## Where application_token Is Passed

Excerpt of the POST request for the `ONAPPINSTALL` event:

```json
{
    "event": "ONAPPINSTALL",
    "data": {
        "VERSION": "1.0.0",
        "ACTIVE": "Y",
        "INSTALLED": "Y",
        "LANGUAGE_ID": "en"
    },
    "ts": "1696527000",
    "auth": {
        "domain": "some-domain.bitrix24.com",
        "scope": "crm,user,task",
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "expires_in": 3600,
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "L",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

Retain the `auth` keys required to verify incoming events.

#|
|| **Key**
`type` | **What to Retain** | **How to Use** ||
|| **auth.application_token**
[`string`](../data-types.md) | For an application, retain the value when handling the [`ONAPPINSTALL`](../common/events/on-app-install.md) event and update it when handling the [`ONAPPUPDATE`](../common/events/on-app-update.md) event. For an outgoing webhook, retain the value of the *Application token* field | Compare it with `auth.application_token`, which is passed to each event handler ||
|| **auth.member_id**
[`string`](../data-types.md) | Retain the Bitrix24 ID together with the token | Use it to find the retained token if one handler receives events from several Bitrix24 accounts ||
|#

The full set of `auth` keys is described in the [general auth parameter table](./index.md#auth).

{% note warning "" %}

Treat `application_token` as a secret: do not write it to logs, pass it to client-side code, or publish it in error messages.

{% endnote %}

## How to Verify the Token in a Handler

1. Retrieve the retained `application_token` by `auth.member_id`
2. Compare the retained token with `auth.application_token` from the incoming event
3. If the tokens do not match, return code `403` and stop processing the event

Handler example:

```php
$auth = $_POST['auth'] ?? [];
$memberId = $auth['member_id'] ?? '';
$incomingApplicationToken = $auth['application_token'] ?? '';

$storedApplicationToken = getStoredApplicationToken($memberId);

if ($storedApplicationToken === '' || $incomingApplicationToken !== $storedApplicationToken) {
    http_response_code(403);
    exit;
}

handleBitrix24Event($_POST);
```

## Continue Learning

- [{#T}](./events.md)
- [{#T}](./index.md#auth)
- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./offline-events.md)
- [{#T}](../common/events/on-app-install.md)
- [{#T}](../common/events/on-app-update.md)
- [{#T}](../common/events/on-app-uninstall.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)

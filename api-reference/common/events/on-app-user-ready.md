# Event for Creating an Application System User ONAPPUSERREADY

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: the application handler is registered automatically

The `ONAPPUSERREADY` event is triggered after the application installation is completed successfully, when Bitrix24 has created or reactivated the application [system user](../../../settings/system-user.md).

The event handler is registered automatically at the application handler URL used for installation. The handler is transferred together with the application configuration.

Unlike [`ONAPPINSTALL`](./on-app-install.md), the `ONAPPUSERREADY` event is sent for both immediate and deferred installation completion modes and passes long-lived authorization for the system user. If the application needs to run without an employee's involvement, rely on the `ONAPPUSERREADY` event.

## What the Handler Receives

Data is passed as a POST request in form-encoded format {.b24-info}

```json
{
    "event": "ONAPPUSERREADY",
    "event_handler_id": "17",
    "data": {
        "access_token": "s1a2b3c4d5e6f70890abcdef1234567890abcd",
        "refresh_token": "r1a2b3c4d5e6f70890abcdef1234567890abcd",
        "expires_in": "3600",
        "scope": "crm,user,task",
        "domain": "oauth.bitrix.info",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a1b2c3d4e5f60718293a4b5c6d7e8f90",
        "user_id": "512",
        "client_id": "app.573ad8a0346747.09223434",
        "status": "L",
        "LANGUAGE_ID": "de"
    },
    "ts": "1756890123",
    "auth": {
        "access_token": "u9z8y7x6w5v4u3t2s1r0q9p8o7n6m5l4",
        "refresh_token": "q9z8y7x6w5v4u3t2s1r0q9p8o7n6m5l4",
        "expires_in": "3600",
        "scope": "crm,user,task",
        "domain": "oauth.bitrix.info",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a1b2c3d4e5f60718293a4b5c6d7e8f90",
        "user_id": "1",
        "status": "L",
        "application_token": "0f1e2d3c4b5a69788796a5b4c3d2e1f0"
    }
}
```

{% note info "" %}

The `data` object contains the system user authorization, and the `auth` object contains the authorization of the employee who installed the application and the `application_token` for event verification.

{% endnote %}

## Request Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic event code.

In this case — `ONAPPUSERREADY` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Event handler ID ||
|| **data***
[`object`](../../data-types.md) | Object with system user authorization parameters.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time of the event sent from the [event queue](../../events/index.md) ||
|| **auth***
[`object`](../../data-types.md) | Object containing authorization parameters and information about the Bitrix24 account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Name**
`type` | **Description** ||
|| **access_token***
[`string`](../../data-types.md) | System user access token ||
|| **refresh_token***
[`string`](../../data-types.md) | Token for refreshing the system user authorization ||
|| **expires_in***
[`integer`](../../data-types.md) | Access token lifetime in seconds ||
|| **scope***
[`string`](../../data-types.md) | List of permissions granted to the application ||
|| **domain***
[`string`](../../data-types.md) | Authorization server domain ||
|| **server_endpoint***
[`string`](../../data-types.md) | Authorization server address for refreshing OAuth 2.0 tokens ||
|| **client_endpoint***
[`string`](../../data-types.md) | Base path for calling Bitrix24 API methods ||
|| **member_id***
[`string`](../../data-types.md) | Bitrix24 account ID ||
|| **user_id***
[`integer`](../../data-types.md) | System user ID in Bitrix24 ||
|| **client_id***
[`string`](../../data-types.md) | Application ID ||
|| **status***
[`string`](../../data-types.md) | Application status.

Possible values:

- `L` — local application
- `S`, `T`, `D`, `P` — mass-market application variants
||
|| **LANGUAGE_ID***
[`string`](../../data-types.md) | Bitrix24 language at the time of application installation ||
|| **date_finish**
[`timestamp`](../../data-types.md) | Subscription end date and time, if known to Bitrix24 ||
|#

{% note info "" %}

The `APP_ID` field is not passed in `data`. The application identifies itself by `client_id` and `member_id`.

{% endnote %}

### Parameter auth {#auth}

#|
|| **Name**
`type` | **Description** ||
|| **access_token***
[`string`](../../data-types.md) | Token for API calls ||
|| **refresh_token***
[`string`](../../data-types.md) | Token for refreshing OAuth 2.0 authorization ||
|| **expires_in***
[`integer`](../../data-types.md) | Access token lifetime in seconds ||
|| **scope***
[`string`](../../data-types.md) | List of permissions granted to the application ||
|| **domain***
[`string`](../../data-types.md) | Address of the Bitrix24 account where the event occurred ||
|| **server_endpoint***
[`string`](../../data-types.md) | Bitrix24 authorization server address required to refresh OAuth 2.0 tokens ||
|| **client_endpoint***
[`string`](../../data-types.md) | Base path for calling Bitrix24 API methods ||
|| **member_id***
[`string`](../../data-types.md) | ID of the Bitrix24 account where the event occurred ||
|| **user_id***
[`integer`](../../data-types.md) | ID of the employee who installed the application ||
|| **status***
[`string`](../../data-types.md) | Application status.

Possible values:

- `L` — local application
- `S`, `T`, `D`, `P` — mass-market application variants
||
|| **application_token***
[`string`](../../data-types.md) | Token for secure event handling ||
|#

## How to Handle the Event

1. Check `auth.application_token`
2. Retain `data.refresh_token`, `data.member_id`, and `data.client_endpoint`
3. Refresh the access token using the standard OAuth refresh flow with the retained `refresh_token`
4. Run background calls to Bitrix24 API methods under the system user authorization

Handler example:

```php
$data = $_POST['data'] ?? [];
$auth = $_POST['auth'] ?? [];

if (($auth['application_token'] ?? '') !== $storedApplicationToken) {
    http_response_code(403);
    exit;
}

saveSystemUserAuth(
    memberId: $data['member_id'],
    domain: $data['client_endpoint'],
    userId: (int)$data['user_id'],
    accessToken: $data['access_token'],
    refreshToken: $data['refresh_token'],
    expiresIn: (int)$data['expires_in'],
);
```

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](../../../settings/system-user.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-uninstall.md)

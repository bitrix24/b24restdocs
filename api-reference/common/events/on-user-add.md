# Event onUserAdd

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onUserAdd` event is triggered when a user is added to Bitrix24. The event occurs not after the invitation, but after the user logs into the account and completes the registration.

## What the handler receives

Data is sent as a POST request {.b24-info}

```
{
    "event": "ONUSERADD",
    "data": {
        "ID": 123,
        "ACTIVE": "Y",
        "EMAIL": "user@example.com",
        "NAME": "John",
        "LAST_NAME": "Doe",
        "PERSONAL_GENDER": "M",
        "PERSONAL_BIRTHDAY": "1990-01-01",
        "UF_DEPARTMENT": [1, 2],
        "DATE_REGISTER": "2024-04-05T10:00:00+02:00",
        "WORK_POSITION": "Developer",
        "UF_EMPLOYMENT_DATE": "2024-04-05"
    },
    "ts": "1466439714",
    "auth": {
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/", 
        "client_endpoint": "https://some-domain.bitrix24.com/rest/", 
        "member_id": "a223c6b3710f85df22e9377d6c4f7553"
    }
}
```

## Request parameters

{% include [Note about required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic event code — `ONUSERADD` ||
|| **data***
[`array`](../../data-types.md) | Data of the added user.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time the event was sent ||
|| **auth***
[`array`](../../data-types.md) | Authorization and account data.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

{% include [Note about required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../data-types.md) | User identifier ||
|| **ACTIVE***
[`string`](../../data-types.md) | Active flag.

Possible values:
- `Y` — yes
- `N` — no ||
|| **EMAIL**
[`string`](../../data-types.md) | User's email ||
|| **NAME**
[`string`](../../data-types.md) | User's first name ||
|| **LAST_NAME**
[`string`](../../data-types.md) | User's last name ||
|| **PERSONAL_GENDER**
[`string`](../../data-types.md) | Gender: `M` — male, `F` — female ||
|| **PERSONAL_BIRTHDAY**
[`string`](../../data-types.md) | Date of birth in `YYYY-MM-DD` format ||
|| **UF_DEPARTMENT**
[`array`](../../data-types.md)\|[`null`](../../data-types.md) | Array of department `ID`s. May be absent for Extranet users ||
|| **DATE_REGISTER***
[`string`](../../data-types.md) | Registration date in `ISO 8601` format ||
|| **WORK_POSITION**
[`string`](../../data-types.md) | User's position ||
|| **UF_EMPLOYMENT_DATE**
[`string`](../../data-types.md) | Employment date in `YYYY-MM-DD` format ||
|#

{% note info "" %}

Some fields may be absent or have a value of `null` if the application does not have access to them.

{% endnote %}

### Parameter auth {#auth}

{% include [Note about required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **access_token***
[`string`](../../data-types.md) | Token for accessing the API ||
|| **expires_in***
[`integer`](../../data-types.md) | Time in seconds until the token expires ||
|| **scope***
[`string`](../../data-types.md) | [Scope](../../scopes/permissions.md) under which the event occurred ||
|| **domain***
[`string`](../../data-types.md) | Address of Bitrix24 where the event occurred ||
|| **server_endpoint***
[`string`](../../data-types.md) | Address of the Bitrix24 authorization server needed to refresh OAuth 2.0 tokens ||
|| **status***
[`string`](../../data-types.md) | Status of the application that subscribed to this event:

- `L` — [local](../../../local-integrations/local-apps.md) application
- `F` — [free mass-market](../../../market/index.md) application

||
|| **client_endpoint***
[`string`](../../data-types.md) | General path for API method calls for Bitrix24 where the event occurred ||
|| **member_id***
[`string`](../../data-types.md) | Identifier of Bitrix24 where the event occurred ||
|| **refresh_token***
[`string`](../../data-types.md) | Token for refreshing authorization [OAuth 2.0](../../../settings/oauth/index.md) ||
|| **application_token***
[`string`](../../data-types.md) | Token for secure event handling ||
|#

## Continue exploring

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./on-app-install.md)
- [{#T}](./on-app-payment.md)
- [{#T}](./on-app-method-confirm.md)
- [{#T}](./on-app-uninstall.md)
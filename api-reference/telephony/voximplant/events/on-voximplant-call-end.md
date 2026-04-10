# Event of Call End ONVOXIMPLANTCALLEND

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONVOXIMPLANTCALLEND` is triggered when a call ends and information about the call is recorded in the history.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONVOXIMPLANTCALLEND",
    "event_handler_id": "1061",
    "data": {
        "CALL_ID": "externalCall.7b0c7de811455ef32b18dc5917e4306a.1773239327",
        "CALL_TYPE": "1",
        "PHONE_NUMBER": "+19061234567",
        "PORTAL_NUMBER": "REST_APP:",
        "PORTAL_USER_ID": "1269",
        "CALL_DURATION": "0",
        "CALL_START_DATE": "2026-03-11T17:28:46+02:00",
        "COST": "0",
        "COST_CURRENCY": "",
        "CALL_FAILED_CODE": "304",
        "CALL_FAILED_REASON": "Call canceled",
        "CRM_ACTIVITY_ID": "7953"
    },
    "ts": "1773239624",
    "auth": {
        "access_token": "s7p6eclrvim9da28dt9ch94ekreb52sv",
        "expires_in": "3600",
        "scope": "telephony",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
        "client_endpoint": "https://example.bitrix24.com/rest/",
        "member_id": "a223c6b9410f85df78e9377d6c4f7213",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "application_token": "51610fefc120afg4b628cc82d6298cce"
    }
}
```

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONVOXIMPLANTCALLEND` ||
|| **event_handler_id**
[`string`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing event data.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object with authorization parameters of the user on behalf of whom the event was triggered.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **CALL_ID**
[`string`](../../../data-types.md) | Identifier of the call ||
|| **CALL_TYPE**
[`integer`](../../../data-types.md) | Type of the call.

Possible values:

- `1` — outgoing
- `2` — incoming
- `3` — incoming with redirection
- `4` — callback
- `5` — informational call ||
|| **PHONE_NUMBER**
[`string`](../../../data-types.md) | Phone number of the subscriber for an incoming call or the number the operator is calling for an outgoing call ||
|| **PORTAL_NUMBER**
[`string`](../../../data-types.md) | Number that received the incoming call or the number from which the outgoing call was made ||
|| **PORTAL_USER_ID**
[`integer`](../../../data-types.md) | Identifier of the operator associated with the call ||
|| **CALL_DURATION**
[`integer`](../../../data-types.md) | Duration of the call in seconds ||
|| **CALL_START_DATE**
[`datetime`](../../../data-types.md) | Date and time the call started in ISO-8601 ||
|| **COST**
[`double`](../../../data-types.md) | Cost of the call ||
|| **COST_CURRENCY**
[`string`](../../../data-types.md) | Currency of the call ||
|| **CALL_FAILED_CODE**
[`string`](../../../data-types.md) | Call completion code.

Possible values:

- `200` — successful call
- `304` — missed call
- `603` — declined
- `603-S` — call canceled
- `403` — forbidden
- `404` — invalid number
- `486` — busy
- `484` — direction unavailable
- `503` — direction unavailable
- `480` — temporarily unavailable
- `402` — insufficient funds
- `423` — blocked
- `OTHER` — undefined ||
|| **CALL_FAILED_REASON**
[`string`](../../../data-types.md) | Text description of the call completion code ||
|| **CRM_ACTIVITY_ID**
[`integer`](../../../data-types.md) | Identifier of the CRM activity associated with the call ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue exploring

- [{#T}](./on-voximplant-call-init.md)
- [{#T}](./on-voximplant-call-start.md)
- [{#T}](../voximplant-statistic-get.md)
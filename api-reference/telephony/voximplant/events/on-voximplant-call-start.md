# Event of Call Start ONVOXIMPLANTCALLSTART

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONVOXIMPLANTCALLSTART` is triggered at the beginning of a conversation: when the operator answers an incoming call or the subscriber answers an outgoing call.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the handler receives

Data is transmitted in the form of a POST request {.b24-info}

```json
{
    "event": "ONVOXIMPLANTCALLSTART",
    "event_handler_id": "1059",
    "data": {
        "CALL_ID": "externalCall.7b0c7de811455ef32b18dc5917e4306a.1773239327",
        "USER_ID": "1269"
    },
    "ts": "1773239326",
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

In this case — `ONVOXIMPLANTCALLSTART` ||
|| **event_handler_id**
[`string`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing event data.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object with user authorization parameters on behalf of which the event was triggered.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **CALL_ID**
[`string`](../../../data-types.md) | Identifier of the call ||
|| **USER_ID**
[`integer`](../../../data-types.md) | Identifier of the user who answered the call ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue your exploration

- [{#T}](./on-voximplant-call-init.md)
- [{#T}](./on-voximplant-call-end.md)
- [{#T}](../voximplant-statistic-get.md)
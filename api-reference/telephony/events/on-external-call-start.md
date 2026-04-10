# Event of Outgoing Call Start ONEXTERNALCALLSTART

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONEXTERNALCALLSTART` event is triggered when a user clicks on a phone number in CRM entities to make an outgoing call through the selected telephony application.

To ensure the event is triggered, specify the application in the Default Outgoing Call Number field in the telephony settings or select it as the default application in the user settings.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONEXTERNALCALLSTART",
    "event_handler_id": "1055",
    "data": {
        "PHONE_NUMBER": "+14155551234",
        "PHONE_NUMBER_INTERNATIONAL": "+14155551234",
        "EXTENSION": "",
        "USER_ID": "1269",
        "CALL_LIST_ID": "0",
        "LINE_NUMBER": "7",
        "IS_MOBILE": "0",
        "CALL_ID": "externalCall.89350264e026dace3f2b14d1f438257c.1773236989",
        "CRM_ENTITY_TYPE": "CONTACT",
        "CRM_ENTITY_ID": "797"
    },
    "ts": "1773236988",
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
[`string`](../../data-types.md) | Symbolic event code.

In this case — `ONEXTERNALCALLSTART` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../data-types.md) | Object containing event data.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../data-types.md) | Date and time the event was sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`](../../data-types.md) | Object containing the user's authorization parameters under which the event was triggered.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **PHONE_NUMBER**
[`string`](../../data-types.md) | The phone number from which the user initiated the outgoing call ||
|| **PHONE_NUMBER_INTERNATIONAL**
[`string`](../../data-types.md) | The phone number in international format ||
|| **EXTENSION**
[`string`](../../data-types.md) | Extension number ||
|| **USER_ID**
[`integer`](../../data-types.md) | Identifier of the user for whom the external call needs to be registered ||
|| **CALL_LIST_ID**
[`integer`](../../data-types.md) | Identifier of the call list if the call was initiated from a call list ||
|| **LINE_NUMBER**
[`string`](../../data-types.md) | Number of the external line through which the call was requested ||
|| **IS_MOBILE**
[`string`](../../data-types.md) | Flag indicating that the call was initiated from a mobile application.

Possible values:
- `0` - the call was not initiated from a mobile application
- `1` - the call was initiated from a mobile application ||
|| **CALL_ID**
[`string`](../../data-types.md) | Identifier of the call created when registering the external call ||
|| **CRM_ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the CRM entity from which the call was initiated ||
|| **CRM_ENTITY_ID**
[`integer`](../../data-types.md) | Identifier of the CRM entity whose type is specified in `CRM_ENTITY_TYPE` ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../_includes/auth-params-in-events.md) %}

## Continue your exploration

- [{#T}](../telephony-external-call-register.md)
- [{#T}](../telephony-external-call-finish.md)
- [{#T}](./on-external-call-back-start.md)
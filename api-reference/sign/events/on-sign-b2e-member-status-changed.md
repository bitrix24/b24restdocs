# Event on Participant Status Change OnSignB2eMemberStatusChanged

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sign.b2e`](../../scopes/permissions.md)
>
> Who can subscribe: a user with access to KEDO

The event `OnSignB2eMemberStatusChanged` is triggered when the status of a signing participant changes.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONSIGNB2EMEMBERSTATUSCHANGED",
    "event_handler_id": "1211",
    "data": {
        "memberUid": "2ditmm6vjoolzbygaeua2jltkjb9hgks",
        "documentUid": "R-LK-50JI-3AAK-WS1A",
        "statusCode": "done",
        "statusName": "Signed"
    },
    "ts": "1770374945",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "sign.b2e",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "L",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "60133c09d1f5d0fd6d7884a11fad4585",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "application_token": "81905784dd6e05280c9a2015e0e61e68"
    }
}
```

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../data-types.md) | Symbolic event code.

In this case — `ONSIGNB2EMEMBERSTATUSCHANGED` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../data-types.md) | Object containing information about the participant ||
|| **data.memberUid**
[`string`](../../data-types.md) | Unique identifier of the participant ||
|| **data.documentUid**
[`string`](../../data-types.md) | Unique identifier of the document ||
|| **data.companyUid**
[`string`](../../data-types.md) | Unique identifier of the company. Returned in the event if the document is signed with the company for which the integration was created ||
|| **data.statusCode**
[`string`](../../data-types.md) | Status code of the participant ||
|| **data.statusName**
[`string`](../../data-types.md) | Name of the participant's status ||
|| **ts**
[`timestamp`](../../data-types.md) | Date and time the event was sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`](../../data-types.md) | Object with authorization parameters and portal data.

The structure is described [below](#auth) ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./on-sign-b2e-document-status-changed.md)
# Event When an HCM Link Document Is Signed OnSignHcmLinkB2eDocumentSigned

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sign.b2e`](../../scopes/permissions.md)
>
> Who can subscribe: a user with access to e-Signature for HR

The `ONSIGNHCMLINKB2EDOCUMENTSIGNED` event is triggered after an e-Signature for HR document linked to HCM Link is signed.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONSIGNHCMLINKB2EDOCUMENTSIGNED",
    "event_handler_id": "1215",
    "data": {
        "id": 3942,
        "company": "acme-hr"
    },
    "ts": "1786086930",
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
[`string`](../../data-types.md) | Symbolic code of the event.

In this case — `ONSIGNHCMLINKB2EDOCUMENTSIGNED` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../data-types.md) | Object containing data about the signed HCM Link document ||
|| **data.id**
[`integer`](../../data-types.md) | Signing participant identifier. Pass it in the `id` parameter of the [sign.b2e.hcmlink.document.get](../sign-b2e-hcmlink-document-get.md) method to retrieve signed document data ||
|| **data.company**
[`string`](../../data-types.md) | HCM Link company code ||
|| **ts**
[`timestamp`](../../data-types.md) | Date and time of the event sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`](../../data-types.md) | Object with authorization parameters and Bitrix24 data.

The structure is described [below](#auth) ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](../sign-b2e-hcmlink-document-get.md)
- [{#T}](./on-sign-b2e-document-status-changed.md)
- [{#T}](./on-sign-b2e-member-status-changed.md)

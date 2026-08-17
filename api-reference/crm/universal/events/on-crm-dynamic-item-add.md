# Event for Creating a Custom Type CRM Object onCrmDynamicItemAdd

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONCRMDYNAMICITEMADD` event is triggered when an item of any [custom object type](../user-defined-object-types/index.md) is added to the CRM.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

Event for creating a smart process item with `entityTypeId = 1220`:

```json
{
    "event": "ONCRMDYNAMICITEMADD",
    "event_handler_id": "4",
    "data": {
        "FIELDS": {
            "ID": "22",
            "ENTITY_TYPE_ID": "1220"
        }
    },
    "ts": "1723534033",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "crm",
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

#|
|| **parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMDYNAMICITEMADD` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing information about the created custom type CRM object.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../data-types.md) | An object containing information about the fields of a created CRM custom type object.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the created custom type CRM object ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the custom CRM type ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

{% note warning "System Object Type Events" %}

Although [universal CRM methods](../index.md) allow adding and modifying objects of standard types such as deals, leads, contacts, companies, and estimates, the `onCrmDynamicItemAdd` event will not trigger when adding the listed objects.

To track new deals, leads, and so on, you can use specific events:

- [{#T}](../../deals/events/on-crm-deal-add.md)
- [{#T}](../../leads/events/on-crm-lead-add.md)
- [{#T}](../../contacts/events/on-crm-contact-add.md)
- [{#T}](../../companies/events/on-crm-company-add.md)
- [{#T}](../../quote/events/on-crm-quote-add.md)

The `onCrmDynamicItemAdd` event will trigger when new invoices are added.

{% endnote %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](index.md)
- [{#T}](on-crm-dynamic-item-update.md)
- [{#T}](on-crm-dynamic-item-delete.md)

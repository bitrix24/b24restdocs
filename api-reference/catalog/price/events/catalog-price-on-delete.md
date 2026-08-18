# Event on Price Deletion CATALOG.PRICE.ON.DELETE

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `CATALOG.PRICE.ON.DELETE` event is triggered when a price is deleted.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "CATALOG.PRICE.ON.DELETE",
    "event_handler_id": 1,
    "data": {
        "FIELDS": {
            "ID": 1
        }
    },
    "ts": 1714649632,
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": 3600,
        "scope": "catalog",
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

## Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `CATALOG.PRICE.ON.DELETE` ||
|| **event_handler_id***
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data***
[`object`](../../../data-types.md) | An object containing information about the remote price.

Contains a single key `FIELDS` ||
|| **data.FIELDS***
[`object`](../../../data-types.md) | An object containing information about price fields.

The structure is described [below](#fields) ||
|| **ts***
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth***
[`object`](../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`catalog_price.id`](../../data-types.md#catalog_price) | Price identifier. You can retrieve all price fields by its identifier using the [catalog.price.get](../catalog-price-get.md) method ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./catalog-price-on-add.md)
- [{#T}](./catalog-price-on-update.md)
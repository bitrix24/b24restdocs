# Event on Product Deletion CATALOG.PRODUCT.ON.DELETE

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `CATALOG.PRODUCT.ON.DELETE` event is triggered when a product is deleted.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "CATALOG.PRODUCT.ON.DELETE",
    "event_handler_id": 1,
    "data": {
        "FIELDS": {
            "ID": 1,
            "TYPE": 1
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

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic code of the event.

In this case — `CATALOG.PRODUCT.ON.DELETE` ||
|| **event_handler_id***
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data***
[`object`](../../data-types.md) | An object containing information about a remote product.

Contains a single key `FIELDS` ||
|| **data.FIELDS***
[`object`](../../data-types.md) | An object containing information about product fields.

The structure is described [below](#fields) ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth***
[`object`](../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`catalog_product.id`](../../data-types.md#catalog_product)\|
[`catalog_product_sku.id`](../../data-types.md#catalog_product_sku)\|
[`catalog_product_offer.id`](../../data-types.md#catalog_product_offer)\|
[`catalog_product_service.id`](../../data-types.md#catalog_product_service) | Product identifier. You can retrieve all product fields by its identifier using the methods:
- [catalog.product.get](../catalog-product-get.md) — for simple products
- [catalog.product.sku.get](../sku/catalog-product-sku-get.md) — for parent products
- [catalog.product.offer.get](../offer/catalog-product-offer-get.md) — for variations
- [catalog.product.service.get](../service/catalog-product-service-get.md) — for services
||
|| **TYPE***
[`integer`](../../data-types.md) | Product type:
- `1` — simple product
- `3` — parent product with variations
- `4` — variation
- `5` — variation without a product
- `6` — parent product without variations
- `7` — service
||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./catalog-product-on-add.md)
- [{#T}](./catalog-product-on-update.md)
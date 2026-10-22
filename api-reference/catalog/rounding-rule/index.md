# Price Rounding Rules in the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The price rounding rule is a set of settings that define how to modify the price of a product or service in the cart. They help automatically round prices, for example, to the nearest whole number or to a specific decimal place.

There are three types of price rounding:

- mathematical rounding
- rounding up: the price increases, which benefits the store
- rounding down: the price decreases, which benefits the customer

> Quick navigation: [all methods and events](#all-methods)

## How to Start

1. Get the price type using [catalog.priceType.list](../price-type/catalog-price-type-list.md)
2. Check available rule fields using [catalog.roundingRule.getFields](./catalog-rounding-rule-get-fields.md)
3. Create a rounding rule using [catalog.roundingRule.add](./catalog-rounding-rule-add.md)
4. Check the rule using [catalog.roundingRule.get](./catalog-rounding-rule-get.md) or [catalog.roundingRule.list](./catalog-rounding-rule-list.md)
5. Track changes through [rounding rule events](./events/index.md)

## Rounding Rules in REST 3.0

In [REST 3.0](../../rest-v3.md), rounding rules have methods for adding, updating, retrieving, and deleting, as well as the `catalog.roundingrule.field.list` and `catalog.roundingrule.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- method names are written in lowercase: `catalog.roundingrule.list`. Calling `catalog.roundingRule.list` at the `/rest/api/` address returns the `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION` error
- the `price` value must be unique within a price type
- the [catalog.roundingrule.update](./catalog-rounding-rule-update-rest-v3.md) method changes only the passed fields, including the `catalogGroupId` price type

## Relationship with Other Objects

**Price Types.** A rounding rule applies to a price type. You can get and configure price types using [catalog.priceType.*](../price-type/index.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - administrator — for methods of the previous API version
> - a user with the "View product catalog" or "Manage price types" access permission — for retrieving rules in REST 3.0. Adding, updating, and deleting require the "Manage price types" access permission

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [catalog.roundingRule.add](./catalog-rounding-rule-add.md) | Adds a price rounding rule ||
    || [catalog.roundingrule.add](./catalog-rounding-rule-add-rest-v3.md) | Adds a price rounding rule v 3.0 ||
    || [catalog.roundingRule.update](./catalog-rounding-rule-update.md) | Modifies a price rounding rule ||
    || [catalog.roundingrule.update](./catalog-rounding-rule-update-rest-v3.md) | Modifies a price rounding rule v 3.0 ||
    || [catalog.roundingRule.get](./catalog-rounding-rule-get.md) | Returns information about a price rounding rule by its identifier ||
    || [catalog.roundingrule.get](./catalog-rounding-rule-get-rest-v3.md) | Returns information about a price rounding rule by its identifier v 3.0 ||
    || [catalog.roundingRule.list](./catalog-rounding-rule-list.md) | Returns a list of price rounding rules ||
    || [catalog.roundingrule.list](./catalog-rounding-rule-list-rest-v3.md) | Returns a list of price rounding rules v 3.0 ||
    || [catalog.roundingRule.delete](./catalog-rounding-rule-delete.md) | Deletes a price rounding rule ||
    || [catalog.roundingrule.delete](./catalog-rounding-rule-delete-rest-v3.md) | Deletes a price rounding rule v 3.0 ||
    || [catalog.roundingRule.getFields](./catalog-rounding-rule-get-fields.md) | Returns the fields of a price rounding rule ||
    || [catalog.roundingrule.field.list](./catalog-rounding-rule-field-list.md) | Returns a list of price rounding rule fields v 3.0 ||
    || [catalog.roundingrule.field.get](./catalog-rounding-rule-field-get.md) | Returns the description of a price rounding rule field ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [CATALOG.ROUNDING.ON.ADD](./events/catalog-rounding-on-add.md) | When a price rounding rule is added ||
    || [CATALOG.ROUNDING.ON.UPDATE](./events/catalog-rounding-on-update.md) | When a price rounding rule is updated ||
    || [CATALOG.ROUNDING.ON.DELETE](./events/catalog-rounding-on-delete.md) | When a price rounding rule is deleted ||
    |#

{% endlist %}

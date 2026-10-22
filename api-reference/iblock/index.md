# Information Blocks in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An information block stores elements of the same kind together with sections and properties. [Universal lists](../lists/index.md) and the [Trade Catalog](../catalog/index.md) are built on information blocks. The `iblock.element.*` methods read information block elements with property values and return the description of element fields.

{% note warning "" %}

The `iblock.element.get` and `iblock.element.list` methods work only with information blocks that have an "API symbolic code" set and the "Access via REST is enabled" option selected in the admin panel. Cloud Bitrix24 has no admin panel, so these methods return an error for any information block.

{% endnote %}

> Quick navigation: [all methods](#all-methods)

{% note info "" %}

The methods in this section belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

## Getting Started

1. In the admin panel of the on-premise Bitrix24, open the information block settings, set the "API symbolic code", and select the "Access via REST is enabled" option
2. Check the available element fields using the [iblock.element.field.list](./iblock-element-field-list.md) method
3. Retrieve the information block elements using the [iblock.element.list](./iblock-element-list.md) method, passing the information block identifier in the `iblockId` filter
4. Retrieve a single element using the [iblock.element.get](./iblock-element-get.md) method by the information block and element identifiers

## Limitations and Specifics {#restrictions}

- The "API symbolic code" (`API_CODE`) and "Access via REST is enabled" (`REST_ON`) settings are available only in the information block settings in the admin panel. These settings cannot be changed via REST
- If the information block has no API code, the methods return the `Unable to compile iblock entity.` error. If the code is set but REST access is disabled, they return `Restricted iblock ID:N (code)`. For a nonexistent information block, `Iblock not found.` is returned
- Cloud Bitrix24 has no admin panel, so the `iblock.element.get` and `iblock.element.list` methods do not work in the cloud. Retrieve universal list elements using the [lists.element.get](../lists/elements/lists-element-get.md) method, and catalog products using the [catalog.product.*](../catalog/product/index.md) methods
- The [iblock.element.field.list](./iblock-element-field-list.md) and [iblock.element.field.get](./iblock-element-field-get.md) methods do not access the information block and work in the cloud
- The methods only read data. Information block elements cannot be created, updated, or deleted via `iblock.element.*`
- An element is returned with the `id`, `iblockId`, `name`, `iblockSectionId`, and `properties` fields. For a property, only the value identifier, the value, and the description are returned
- The methods do not check the user's permissions for the information block: access to elements is granted by the "Access via REST is enabled" setting

## Connection with Other Objects

**Universal lists.** Each universal list is an information block, and the list identifier matches the information block identifier. To work with list elements in the cloud, use the [lists.element.*](../lists/elements/index.md) methods.

**Trade Catalog.** The product catalog is also stored in an information block. The catalog information block identifier is returned by the [catalog.catalog.*](../catalog/catalog/index.md) methods, and products are read and updated by the [catalog.product.*](../catalog/product/index.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`iblock`](../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [iblock.element.get](./iblock-element-get.md) | Returns an information block element by identifier ||
|| [iblock.element.list](./iblock-element-list.md) | Returns a list of information block elements by filter ||
|| [iblock.element.field.get](./iblock-element-field-get.md) | Returns the description of an information block element field by name ||
|| [iblock.element.field.list](./iblock-element-field-list.md) | Returns a list of information block element fields ||
|#

## Continue Learning

- [{#T}](./iblock-element-list.md)
- [{#T}](./iblock-element-get.md)
- [{#T}](../lists/index.md)
- [{#T}](../rest-v3.md)

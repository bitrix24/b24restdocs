# Translations of Price Type Names in the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Translating the price type name allows you to set a localized name for each language in Bitrix24. This is necessary to ensure that the same price type is displayed correctly in the interface across different languages.

The translation uses:

- `id` — translation identifier
- `catalogGroupId` — price type identifier
- `lang` — language code
- `name` — localized name of the price type

> Quick navigation: [all methods](#all-methods)

## Considerations Before Calling Methods

- Reading translations requires the "View Product Catalog" or "Manage Price Types" access permission. Creating, updating, and deleting translations requires the "Manage Price Types" access permission

- Only one translation can be created for a specific language for each price type. The pair `catalogGroupId + lang` must be unique. Before creating or updating, check for an existing translation using [catalog.priceTypeLang.list](./catalog-price-type-lang-list.md) with a filter on `catalogGroupId` and `lang`

## How to Start

1. Retrieve the list of available languages using [catalog.priceTypeLang.getLanguages](./catalog-price-type-lang-get-languages.md)
2. Check the field structure using [catalog.priceTypeLang.getFields](./catalog-price-type-lang-get-fields.md)
3. Add a translation using [catalog.priceTypeLang.add](./catalog-price-type-lang-add.md)
4. To modify a translation, use [catalog.priceTypeLang.update](./catalog-price-type-lang-update.md); to delete it, use [catalog.priceTypeLang.delete](./catalog-price-type-lang-delete.md)
5. To read a single translation by `id`, use [catalog.priceTypeLang.get](./catalog-price-type-lang-get.md); to get a list of translations by filter, use [catalog.priceTypeLang.list](./catalog-price-type-lang-list.md)

## Translations in REST 3.0

In [REST 3.0](../../../rest-v3.md), translations have methods for adding, updating, deleting, and retrieving available languages, as well as the `catalog.pricetype.lang.field.list` and `catalog.pricetype.lang.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- the new version has no methods for retrieving a translation or a list of translations. The translation identifier is returned by the [catalog.pricetype.lang.add](./catalog-price-type-lang-add-rest-v3.md) method or by the previous version method [catalog.priceTypeLang.list](./catalog-price-type-lang-list.md)
- a second translation into the same language, or changing `lang` to a language that already has a translation, returns a server error with the HTTP status `500`. Before adding, check the price type translations using the [catalog.priceTypeLang.list](./catalog-price-type-lang-list.md) method
- method names are written in lowercase: `catalog.pricetype.lang.getlanguages`

## Linking Translations to Other Objects

**Price Type.** The translation is linked to the price type through the `catalogGroupId` field. The price type identifier can be obtained using the methods [catalog.priceType.list](../catalog-price-type-list.md) and [catalog.priceType.get](../catalog-price-type-get.md).

**Interface Language.** The language code is used for the translation in the `lang` field. In `lang`, pass the value of the `lid` field from the response of the method [catalog.priceTypeLang.getLanguages](./catalog-price-type-lang-get-languages.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [catalog.priceTypeLang.add](./catalog-price-type-lang-add.md) | Adds a translation for the price type name ||
|| [catalog.pricetype.lang.add](./catalog-price-type-lang-add-rest-v3.md) | Adds a translation for the price type name v 3.0 ||
|| [catalog.priceTypeLang.update](./catalog-price-type-lang-update.md) | Updates the translation for the price type name ||
|| [catalog.pricetype.lang.update](./catalog-price-type-lang-update-rest-v3.md) | Updates the translation for the price type name v 3.0 ||
|| [catalog.priceTypeLang.get](./catalog-price-type-lang-get.md) | Returns the values of the fields for the price type name translation ||
|| [catalog.priceTypeLang.list](./catalog-price-type-lang-list.md) | Returns a list of price type name translations by filter ||
|| [catalog.priceTypeLang.delete](./catalog-price-type-lang-delete.md) | Deletes the translation for the price type name ||
|| [catalog.pricetype.lang.delete](./catalog-price-type-lang-delete-rest-v3.md) | Deletes the translation for the price type name v 3.0 ||
|| [catalog.priceTypeLang.getLanguages](./catalog-price-type-lang-get-languages.md) | Returns the available languages for translation ||
|| [catalog.pricetype.lang.getlanguages](./catalog-price-type-lang-get-languages-rest-v3.md) | Returns the available languages for translation v 3.0 ||
|| [catalog.priceTypeLang.getFields](./catalog-price-type-lang-get-fields.md) | Returns the fields for the price type name translation ||
|| [catalog.pricetype.lang.field.list](./catalog-price-type-lang-field-list.md) | Returns a list of price type name translation fields v 3.0 ||
|| [catalog.pricetype.lang.field.get](./catalog-price-type-lang-field-get.md) | Returns the description of a price type name translation field ||
|#

# Translations of Price Type Names in the Trade Catalog: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Translating the price type name allows you to set a localized name for each language in Bitrix24. This is necessary to ensure that the same price type is displayed correctly in the interface across different languages.

The translation uses:

- `id` — translation identifier
- `catalogGroupId` — price type identifier
- `lang` — language code
- `name` — localized name of the price type

> Quick navigation: [all methods](#all-methods)

## Considerations Before Calling Methods

- The methods `catalog.priceTypeLang.*` are available only to administrators.

- Only one translation can be created for a specific language for each price type. The pair `catalogGroupId + lang` must be unique. Before creating or updating, check for an existing translation using [catalog.priceTypeLang.list](./catalog-price-type-lang-list.md) with a filter on `catalogGroupId` and `lang`.

## How to Work with Price Type Name Translations

1. Retrieve the list of available languages using [catalog.priceTypeLang.getLanguages](./catalog-price-type-lang-get-languages.md).
2. Check the field structure via [catalog.priceTypeLang.getFields](./catalog-price-type-lang-get-fields.md).
3. Add a translation using the method [catalog.priceTypeLang.add](./catalog-price-type-lang-add.md).
4. To modify a translation, use [catalog.priceTypeLang.update](./catalog-price-type-lang-update.md); to delete, use [catalog.priceTypeLang.delete](./catalog-price-type-lang-delete.md).
5. To read a single translation by `id`, use [catalog.priceTypeLang.get](./catalog-price-type-lang-get.md); to get a list of translations by filter, use [catalog.priceTypeLang.list](./catalog-price-type-lang-list.md).

## Linking Translations to Other Objects

**Price Type.** The translation is linked to the price type through the `catalogGroupId` field. The price type identifier can be obtained using the methods [catalog.priceType.list](../catalog-price-type-list.md) and [catalog.priceType.get](../catalog-price-type-get.md).

**Interface Language.** The language code is used for the translation in the `lang` field. In `lang`, pass the value of the `lid` field from the response of the method [catalog.priceTypeLang.getLanguages](./catalog-price-type-lang-get-languages.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

#| 
|| **Method** | **Description** ||
|| [catalog.priceTypeLang.add](./catalog-price-type-lang-add.md) | Adds a translation for the price type name ||
|| [catalog.priceTypeLang.update](./catalog-price-type-lang-update.md) | Updates the translation for the price type name ||
|| [catalog.priceTypeLang.get](./catalog-price-type-lang-get.md) | Returns the values of the fields for the price type name translation ||
|| [catalog.priceTypeLang.list](./catalog-price-type-lang-list.md) | Returns a list of price type name translations by filter ||
|| [catalog.priceTypeLang.delete](./catalog-price-type-lang-delete.md) | Deletes the translation for the price type name ||
|| [catalog.priceTypeLang.getLanguages](./catalog-price-type-lang-get-languages.md) | Returns the available languages for translation ||
|| [catalog.priceTypeLang.getFields](./catalog-price-type-lang-get-fields.md) | Returns the fields for the price type name translation ||
|#

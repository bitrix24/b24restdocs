# Currency Localization in CRM: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Currency localization refers to the rules for writing numbers and placing currency symbols in different languages.

Localizations do not exist on their own: each set of rules belongs to a specific currency of the [Currencies in CRM](../index.md) section and is stored under a language identifier. A single currency can have its own localization for every language of Bitrix24.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [currencies in Bitrix24](https://helpdesk.bitrix24.com/open/8967215/)

## Currency Identifier and Language Code

**Currency identifier.** All methods of this section accept it in the `id` parameter — it is the symbolic currency code according to the ISO 4217 standard, for example, `USD`. The list of codes created in Bitrix24 is returned by the [crm.currency.list](../crm-currency-list.md) method.

**Language code.** A localization is stored separately for each language, for example, `en` or `de`. In the [crm.currency.localizations.set](./crm-currency-localizations-set.md) method, the language code is the key of the `localizations` object; in the [crm.currency.localizations.delete](./crm-currency-localizations-delete.md) method, the codes are passed as an array in the `lids` parameter.

## What a Localization Consists Of

The set of rules for a single language is described by the [`crm_currency_localization`](../../data-types.md#crm_currency_localization) data type:

- `FULL_NAME` — the name of the currency in this language
- `FORMAT_STRING` — the template for displaying the amount, the value is substituted in place of the `#` character, for example, `$ #`
- `DECIMALS` — the number of decimal places
- `DEC_POINT` — the decimal separator
- `THOUSANDS_VARIANT` and `THOUSANDS_SEP` — the thousands separator; when `THOUSANDS_VARIANT` is set, the value of `THOUSANDS_SEP` is ignored
- `HIDE_ZERO` — whether to hide insignificant zeros

The current list of fields with their types is returned by the [crm.currency.localizations.fields](./crm-currency-localizations-fields.md) method. Existing localizations that are not passed to the [crm.currency.localizations.set](./crm-currency-localizations-set.md) method remain unchanged, and default values are applied to languages without their own localization.

## Using Localization

**Bitrix24.** The display of currency depends on the interface language of Bitrix24 in the employee's account. In the German interface, the amount is displayed in the format `12.345,67 €`, while in English it appears as `€ 12,345.67`.

**Bitrix24.Sites.** In the site settings, you can choose the language for standard phrases in the builder blocks. The site language affects the price display format in the store blocks.

{% note tip "User Documentation" %}

- [How to switch the interface language in Bitrix24](https://helpdesk.bitrix24.com/open/17365500/)
- [Site and page settings](https://helpdesk.bitrix24.com/open/21883080/)

{% endnote %}

## Access Permissions

Permissions depend on the operation:

- a user with read access to CRM objects can read localizations
- a user with the "Allow to modify settings" permission in CRM can change and delete localizations

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [crm.currency.localizations.set](./crm-currency-localizations-set.md) | Updates currency localizations or adds them if there is no localization for the specified language ||
|| [crm.currency.localizations.get](./crm-currency-localizations-get.md) | Retrieves existing currency localizations ||
|| [crm.currency.localizations.delete](./crm-currency-localizations-delete.md) | Deletes currency localizations for specified languages ||
|| [crm.currency.localizations.fields](./crm-currency-localizations-fields.md) | Retrieves the currency localization fields — the settings that depend on the language ||
|#

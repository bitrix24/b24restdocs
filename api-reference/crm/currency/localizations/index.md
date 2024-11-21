# Currency Localization in CRM: Overview of Methods

Currency localization refers to the rules for writing numbers and placing currency symbols in different languages.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [currencies in Bitrix24](https://helpdesk.bitrix24.com/open/8967215/), [localization structure](../../data-types.md#crm_currency_localization) 

## Using Localization

**Bitrix24.** The display of currency depends on the interface language of Bitrix in the employee's account. In the Russian interface, the amount is displayed in the format `12 345,67 ₽`, while in English it appears as `₽ 12,345.67`.

**Bitrix24.Sites.** In the site settings, you can choose the language for standard phrases in the builder blocks. The site language affects the price display format in the store blocks.

{% note tip "User Documentation" %}

- [How to switch the interface language in Bitrix24](https://helpdesk.bitrix24.com/open/17365500/)
- [Site and page settings](https://helpdesk.bitrix24.com/open/21883080/)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method


#|
|| **Method** | **Description** ||
|| [crm.currency.localizations.get](./crm-currency-localizations-get.md) | Retrieves existing currency localizations ||
|| [crm.currency.localizations.set](./crm-currency-localizations-set.md) | Updates localizations for a currency or adds them if the localization for the specified language does not exist ||
|| [crm.currency.localizations.delete](./crm-currency-localizations-delete.md) | Deletes currency localizations for specified languages ||
|| [crm.currency.localizations.fields](./crm-currency-localizations-fields.md) | Retrieves available fields for currency localization, i.e., settings that depend on the language ||
|#
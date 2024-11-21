# Currencies in CRM: Overview of Methods

Currency in CRM helps manage interactions with clients from different countries. For instance, the primary currency in which a company conducts transactions is rubles. The manager adds products priced in rubles to a deal, while the client pays in their preferred currency—tenge. The manager simply needs to change the currency of the deal in the detail form—Bitrix will automatically calculate the product prices in tenge based on the exchange rate set in the settings.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [currencies in Bitrix24](https://helpdesk.bitrix24.com/open/8967215/)

## Currency Connection with CRM Objects

**Products.** The price of a product item can be specified through [price methods](../../catalog/price/index.md). All price type fields are composite—the amount and currency are modified separately. To specify or change the currency, use the `currency` parameter and pass the currency identifier in the format `RUB`.

**Standard field "Amount and Currency."** This field displays the total amount of products and the currency in [deals](../deals/index.md), [leads](../leads/index.md), [invoices](../universal/invoice.md), [estimates](../quote/index.md), and [SPAs](../universal/index.md). The `Amount and Currency` field is composite, with the amount and currency changing separately. To specify or change the currency, use the `CURRENCY_ID` parameter and pass the currency identifier in the format `RUB`.

**Custom field of type "Money."** To specify or change the currency value, pass the amount and currency code in the format `100|RUB`.

## Base Currency

The base currency is the currency in which the company conducts transactions.

* If you create or modify a CRM element without passing the currency code, the base currency is set automatically.
* If you change the currency in the field, the amount will be recalculated based on the base currency exchange rate.

To find out the base currency, use the method [crm.currency.base.get](./crm-currency-base-get.md). To change the base currency, use the method [crm.currency.base.set](./crm-currency-base-set.md).

## Currency Exchange Rate

The exchange rate of any currency in Bitrix is calculated relative to the base currency.

{% note info " " %}

When you change the base currency, the exchange rate ratios are not automatically recalculated.

{% endnote %}

To change the exchange rate of a currency to the base, use the currency update method—[crm.currency.update](./crm-currency-update.md). Set the ratio in the `AMOUNT` parameter.

## Currency Localization

Currency localization refers to the rules for writing numbers and placing the currency symbol in different languages.

To change or remove currency localization settings, use the group of methods [crm.currency.localizations.*](./localizations/index.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.currency.add](./crm-currency-add.md) | Creates a new currency ||
    || [crm.currency.update](./crm-currency-update.md) | Updates an existing currency ||
    || [crm.currency.get](./crm-currency-get.md) | Returns currency by symbolic identifier ||
    || [crm.currency.list](./crm-currency-list.md) | Returns a list of currencies ||
    || [crm.currency.delete](./crm-currency-delete.md) | Deletes a currency ||
    || [crm.currency.fields](./crm-currency-fields.md) | Returns the description of currency fields ||
    || [crm.currency.base.get](./crm-currency-base-get.md) | Gets the symbolic identifier of the base currency ||
    || [crm.currency.base.set](./crm-currency-base-set.md) | Changes the base currency ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmCurrencyAdd](./events/on-crm-currency-add.md) | When a currency is created ||
    || [onCrmCurrencyUpdate](./events/on-crm-currency-update.md) | When a currency is updated ||
    || [onCrmCurrencyDelete](./events/on-crm-currency-delete.md) | When a currency is deleted ||
    |#

{% endlist %}
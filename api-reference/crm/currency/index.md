# Currencies in CRM: Overview of Methods and Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Currency in CRM helps to work with clients from different countries. For example, the primary currency in which the company conducts transactions is rubles. The manager adds products priced in dollars to the deal, while the client pays in their preferred currency—tenge. The manager simply needs to change the currency of the deal in the detail form—Bitrix24 will automatically calculate the cost of the products in tenge based on the exchange rate set in the settings.

The rules for displaying a currency in different languages are configured by the [currency localization](./localizations/index.md) methods, and an application can track currency changes through [currency events](./events/index.md).

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [currencies in Bitrix24](https://helpdesk.bitrix24.com/open/8967215/)

## Currency Identifier

A currency has no numeric identifier. The currency identifier is a symbolic code in the `CURRENCY` field according to the ISO 4217 standard, for example, `USD` or `EUR`. The code is assigned when the currency is created using the [crm.currency.add](./crm-currency-add.md) method and is then passed to every method in this section.

Where to obtain the currency code and the description of its fields:

- the list of codes created in Bitrix24 is returned by the [crm.currency.list](./crm-currency-list.md) method
- the description of currency fields is returned by the [crm.currency.fields](./crm-currency-fields.md) method
- the structure of the currency object is described in the [`crm_currency`](../data-types.md#crm_currency) data type

## Currency Relation to CRM Objects

**Products.** The price of a product item can be specified through [price methods](../../catalog/price/index.md). All price type fields are composite—the amount and currency are changed separately. To specify or change the currency, use the `currency` parameter and pass the currency identifier in the format `USD`.

**Standard field "Amount and Currency."** This field shows the total amount of products and the currency in [deals](../deals/index.md), [leads](../leads/index.md), [invoices](../universal/invoice.md), [estimates](../quote/index.md), and [SPAs](../universal/index.md). The "Amount and Currency" field is composite, with the amount and currency changing separately. To specify or change the currency, use the `CURRENCY_ID` parameter and pass the currency identifier in the format `USD`.

**Custom field of type "Money."** To specify or change the currency value, pass the amount and currency code in the format `100|USD`.

## Base Currency

The base currency is the currency in which the company conducts transactions.

- If you create or modify a CRM object without passing the currency code, the base currency is set automatically.
- If you change the currency in the field, the amount will be recalculated based on the base currency exchange rate.

To find out the base currency, use the method [crm.currency.base.get](./crm-currency-base-get.md). To change the base currency, use the method [crm.currency.base.set](./crm-currency-base-set.md).

## Currency Exchange Rate

The exchange rate of any currency in Bitrix24 is calculated in relation to the base currency.

To change the exchange rate of a currency to the base, use the currency update method—[crm.currency.update](./crm-currency-update.md). Set the ratio using a pair of parameters: `AMOUNT` is the exchange rate to the base currency, and `AMOUNT_CNT` is the nominal value for which this rate is specified.

{% note warning "" %}

When you change the base currency, the exchange rate ratios are not recalculated automatically. The rates of the remaining currencies have to be set again.

{% endnote %}

## Currency Localization

Currency localization refers to the rules for writing numbers and placing the currency symbol in different languages.

To retrieve, change, or remove currency localization settings, use the group of methods [crm.currency.localizations.*](./localizations/index.md).

## How to Track Currency Changes

An application can receive notifications about the creation, update, and deletion of currencies. The list of events and the subscription procedure are described in the [Overview of Events When Working with Currencies](./events/index.md) section.

## Access Permissions

Permissions depend on the operation:

- a user with read access to CRM objects can read currencies and their localizations
- a user with the "Allow to modify settings" permission in CRM can create, modify, and delete currencies, change the base currency, and manage localizations

## Overview of Methods and Events {#all-methods}

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

    The settings for displaying a currency in different languages are described in the [Currency Localization](./localizations/index.md) subsection.

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmCurrencyAdd](./events/on-crm-currency-add.md) | When a currency is added manually or via the method [crm.currency.add](./crm-currency-add.md) ||
    || [onCrmCurrencyUpdate](./events/on-crm-currency-update.md) | When a currency is changed manually or via the method [crm.currency.update](./crm-currency-update.md) ||
    || [onCrmCurrencyDelete](./events/on-crm-currency-delete.md) | When a currency is deleted manually or via the method [crm.currency.delete](./crm-currency-delete.md) ||
    |#

{% endlist %}

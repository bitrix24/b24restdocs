# Currencies in CRM: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Currency in CRM helps to work with clients from different countries. For example, the company conducts transactions in euros, while the client pays in US dollars. The manager adds products priced in euros to the deal and changes the deal currency to US dollars in the detail form. Bitrix24 recalculates the cost of the products in US dollars based on the exchange rate from the settings.

The `crm.currency.*` methods create and delete currencies and set their exchange rates and the base currency.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [currencies in Bitrix24](https://helpdesk.bitrix24.com/open/8967215/)

## Currency Identifier

A currency has no numeric identifier. It is identified by a code of three Latin letters, for example, `USD` or `EUR`. Usually, this is an ISO 4217 code. Method responses return it in the `CURRENCY` field. The code is assigned when the currency is created using the [crm.currency.add](./crm-currency-add.md) method and is passed to methods that work with a specific currency.

Where to obtain the currency code and the description of its fields:

- the list of codes created in Bitrix24 is returned by the [crm.currency.list](./crm-currency-list.md) method
- the description of currency fields is returned by the [crm.currency.fields](./crm-currency-fields.md) method
- the structure of the currency object is described in the [`crm_currency`](../data-types.md#crm_currency) data type

## Currency Relation to CRM Objects

**Products.** The price of a product in the catalog is set by [price methods](../../catalog/price/index.md). The price and currency are passed in separate parameters: the currency code goes in `currency`, for example, `USD`.

**Standard field "Amount and Currency."** This field shows the total amount of products and the currency in [deals](../deals/index.md), [leads](../leads/index.md), [invoices](../universal/invoice.md), [estimates](../quote/index.md), and [SPAs](../universal/index.md). The amount and currency in this field are also passed in separate parameters: the currency code goes in `CURRENCY_ID`, for example, `USD`.

**Custom field of type "Money."** The currency is set and changed together with the amount: pass a value in the format `100|USD` to the field.

## Base Currency

The base currency is the currency in which the company conducts transactions.

If you create a CRM item without passing the currency code, Bitrix24 uses the base currency. If you do not pass the code when updating an item, the currency stays the same.

When the currency code is changed, the methods do not recalculate the amount. If you pass `CURRENCY_ID` = `USD`, the amount `1000` stays `1000`, and product prices do not change either. Pass the amount and prices again in the new currency.

You can retrieve the base currency using the [crm.currency.base.get](./crm-currency-base-get.md) method and change it using the [crm.currency.base.set](./crm-currency-base-set.md) method.

## Currency Exchange Rate

The exchange rate of any currency in Bitrix24 is set relative to the base currency.

You can change the exchange rate using the [crm.currency.update](./crm-currency-update.md) method. The rate is described by two parameters: `AMOUNT_CNT` is the nominal value, that is, the number of currency units, and `AMOUNT` is the value of this nominal amount in the base currency. For example, the base currency is the euro. For the yuan `CNY`, the values `AMOUNT_CNT` = `1` and `AMOUNT` = `0.1220` mean that one yuan costs 0.1220 euros.

{% note warning "" %}

When the base currency is changed, Bitrix24 does not recalculate the exchange rates of other currencies, so set them again. Bitrix24 sets the rate of the new base currency to `1` and does not retain its previous rate. If you later switch back to the previous base currency, you will also need to set the rate of this currency again.

{% endnote %}

## Currency Localization

Currency localization refers to the rules for writing numbers and placing the currency symbol in different languages. You can retrieve, change, or remove localizations using the [crm.currency.localizations.*](./localizations/index.md) methods.

## How to Track Currency Changes

An application can receive notifications about the creation, update, and deletion of currencies. The list of events and the subscription procedure are described in the [Overview of Events When Working with Currencies](./events/index.md) section.

## Errors When Working with Currencies

Typical errors and limitations of the methods in this section:

- a currency code consists of three Latin letters, for example, `USD`. If the code has a different format or such a currency already exists, the [crm.currency.add](./crm-currency-add.md) method returns the `ERROR_CORE` error
- the base currency and the reporting currency cannot be deleted. To delete the base currency, first make another currency the base one using the [crm.currency.base.set](./crm-currency-base-set.md) method
- the [crm.currency.base.set](./crm-currency-base-set.md) method called with the code of a non-existent currency does not change the base currency and returns `false` without an error. Check `result`
- without the required permission, the methods return the `Access denied.` error with an empty code

Error codes and texts for each method are listed on its page, and general REST errors are in the [Error Codes](../../../error-codes.md) section.

## Access Permissions

Permissions depend on the operation:

- a user with read access to at least one CRM object can read currencies and their localizations
- a user with the "Edit CRM preferences" permission in CRM can create, modify, and delete currencies, change the base currency, and manage localizations

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
    || [crm.currency.get](./crm-currency-get.md) | Returns currency parameters by code ||
    || [crm.currency.list](./crm-currency-list.md) | Returns a list of currencies ||
    || [crm.currency.delete](./crm-currency-delete.md) | Deletes a currency ||
    || [crm.currency.base.get](./crm-currency-base-get.md) | Returns the code of the base currency ||
    || [crm.currency.base.set](./crm-currency-base-set.md) | Changes the base currency ||
    || [crm.currency.fields](./crm-currency-fields.md) | Returns the description of currency fields ||
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

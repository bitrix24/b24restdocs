# Autofilling details in the CRM card

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement, crm`](../../../scopes/permissions.md)
>
> Quick navigation: [All Embedding Points](#all-placements)

Autofill points for details connect an application to data search within a CRM card. The application works as an external source: it receives a search string, returns found options, and passes data from the selected option for insertion into the card.

The embedding point code is passed in the `PLACEMENT` parameter of the [placement.bind](../../placement-bind.md) method. If the application works only with specific countries, pass their identifiers separated by commas without spaces in the `OPTIONS[countries]` parameter. Country identifiers are returned by the method [crm.requisite.preset.countries](../../../crm/requisites/presets/crm-requisite-preset-countries.md).

Bitrix24 calls the external search when a user has entered at least three characters into the search string.

The `crmShowFoundEntities` and `crmShowCreatedEntity` commands are called via [BX24.placement.call](../../ui-interaction/bx24-placement-call.md), and the `onCrmEntityIsNeedToCreate` event is handled via [BX24.placement.bindEvent](../../ui-interaction/bx24-placement-bind-event.md). There is no need to register them separately.

{% note info "" %}

The handler will not be available in the search source selection interface until the application installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## How to choose an embedding point {#all-placements}

#|
|| **Embedding point** | **Code** | **When to use** ||
|| [Client details autofill](./requisite-autocomplete.md) | `CRM_REQUISITE_AUTOCOMPLETE` | To search for and substitute company or contact details ||
|| [Bank details autofill](./bank-detail-autocomplete.md) | `CRM_BANK_DETAIL_AUTOCOMPLETE` | To search for and substitute bank details, e.g., by BIC ||
|#

## Workflow

1. The user enters a string in the details autofill field
2. Bitrix24 calls the application handler and passes the search string to `PLACEMENT_OPTIONS.searchQuery`
3. The application passes the found options using the `crmShowFoundEntities` command
4. If the user selects an option, the application receives the `onCrmEntityIsNeedToCreate` event and passes the data for insertion using the `crmShowCreatedEntity` command

## What the Handler Receives

For both points, Bitrix24 passes the same context to the handler: `PLACEMENT_OPTIONS` contains the search string `searchQuery` and the `URI` of the page that initiated the search. The value is passed as a JSON string. After parsing, it looks like this — an example for a client details search from a company card:

```json
{
    "searchQuery": "DE123456789",
    "URI": "/crm/company/details/2979/?any=details%2F2979%2F&IFRAME=Y&IFRAME_TYPE=SIDE_SLIDER"
}
```

For bank details, `URI` contains the address of the details form, not the card. The other request parameters for the handler are standard for widgets, and their structure is described on the embedding point pages.

## How to Return the Found Options

The application passes the found options using the [BX24.placement.call](../../ui-interaction/bx24-placement-call.md) command with the name `crmShowFoundEntities`. In the `data` field, pass an array of options: `id` is the option identifier on the application side, and `name` is the name the user will see:

```javascript
BX24.placement.call(
    'crmShowFoundEntities',
    {
        data: [
            { id: 'company-123', name: 'Müller GmbH' },
            { id: 'company-124', name: 'Schmidt GmbH' }
        ]
    }
);
```

The full composition of an option — phone, e-mail, website — and the insertion of the selected option into the card using the `crmShowCreatedEntity` command are described on the pages [client details autofill](./requisite-autocomplete.md) and [bank details autofill](./bank-detail-autocomplete.md).

## Common errors

#|
|| **Error** | **Solution** ||
|| Handler is registered but unavailable during search | Check that the application is installed and that the correct embedding point code is passed in `PLACEMENT` ||
|| Handler is unavailable for the required country | Check the value of `OPTIONS[countries]`. The string must contain country identifiers separated by commas without spaces ||
|| Found options are not displayed | Pass the array of options to the `data` field of the `crmShowFoundEntities` command ||
|| `placement.bind` returns `WRONG_AUTH_TYPE` with the description `Application context required` | Register the placement on behalf of an application. A placement cannot be bound with a webhook ||
|| The option is not substituted into the card after selection | Subscribe to `onCrmEntityIsNeedToCreate` and, after the option is selected, call `crmShowCreatedEntity` with the fields to insert ||
|#

Other registration error codes are listed in the "Possible Error Codes" section of the [placement.bind](../../placement-bind.md) page.

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../../placements.md)
- [{#T}](../../placement-bind.md)
- [{#T}](../../placement-get.md)
- [{#T}](../../placement-unbind.md)
- [{#T}](../../ui-interaction/bx24-placement-call.md)
- [{#T}](../../ui-interaction/bx24-placement-bind-event.md)
- [{#T}](../../../crm/requisites/index.md)

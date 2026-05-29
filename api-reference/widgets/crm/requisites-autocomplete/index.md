# Autofilling details in the CRM card

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)

Autofill points for details connect an application to data search within a CRM card. The application works as an external source: it receives a search string, returns found options, and passes data from the selected option for insertion into the card.

The embedding point code is passed in the `PLACEMENT` parameter of the [placement.bind](../../placement-bind.md) method. If the application works only with specific countries, pass their identifiers separated by commas without spaces in the `OPTIONS[countries]` parameter.

Bitrix24 calls the external search when a user has entered at least three characters into the search string.

The `crmShowFoundEntities` and `crmShowCreatedEntity` commands are called via `BX24.placement.call`, and the `onCrmEntityIsNeedToCreate` event is handled via `BX24.placement.bindEvent`. There is no need to register them separately.

{% note info "" %}

The handler will not be available in the search source selection interface until the application installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## How to choose an embedding point

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

## Common errors

#|
|| **Error** | **How to fix** ||
|| Handler is registered but unavailable during search | Check that the application is installed and that the correct embedding point code is passed in `PLACEMENT` ||
|| Handler is unavailable for the required country | Check the value of `OPTIONS[countries]`. The string must contain country identifiers separated by commas without spaces ||
|| Found options are not displayed | Pass the array of options to the `data` field of the `crmShowFoundEntities` command ||
|| The option is not substituted into the card after selection | Subscribe to `onCrmEntityIsNeedToCreate` and call `crmShowCreatedEntity` after creation ||
|#

## Continue Learning

- [{#T}](../../placement-bind.md)
- [{#T}](../../placement-get.md)
- [{#T}](../../placement-unbind.md)
- [{#T}](../../ui-interaction/bx24-placement-call.md)
- [{#T}](../../ui-interaction/bx24-placement-bind-event.md)
- [{#T}](../../../crm/requisites/index.md)

# How to embed widgets in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Widgets add an application interface to the CRM. The user works with a card in Bitrix24, while the application interface appears inside the card: in a custom lead field or on a separate tab.

Two scenarios are available. The first shows how to add a field to a lead that opens the application interface. The second provides a video tutorial and materials on adding a tab to a CRM card.

Both scenarios require a handler — an application page with a public URL. Bitrix24 opens this URL in the CRM card and passes the call data to the handler. Each scenario specifies which data the handler receives.

> Quick links: [all scenarios](#choose-tutorial)
>
> User documentation: [CRM implementation steps](https://helpdesk.bitrix24.com/open/23477678/)

## Getting Started

1. Determine where the application interface should appear: in a lead field or on a CRM card tab
2. Prepare a handler — an application page with a URL accessible from an external network
3. Find the required scenario in the [How to choose a scenario](#choose-tutorial) table
4. Check which permissions and scope are specified in the chosen scenario
5. Execute the methods in the order described in the scenario
6. Complete the application installation and open a CRM card to verify the handler call

## How to choose a scenario {#choose-tutorial}

#|
|| **If necessary** | **Open** ||
|| Show the application interface inside the lead custom field | [Embed a widget into a lead as a custom property](./widget-as-field-in-lead-page.md) ||
|| Watch the video tutorial and materials on adding a tab to a CRM card | [Embed a widget into a CRM card](./widget-as-detail-tab.md) ||
|| Clarify tab codes and the data received by the handler | [Tab in CRM card CRM_XXX_DETAIL_TAB](../../../api-reference/widgets/crm/detail-tab.md) ||
|| Learn how custom CRM field types work | [Custom field types in CRM](../../../api-reference/crm/universal/user-defined-fields/userfield-type.md) ||
|#

# How to embed widgets in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Widgets add an application interface to the CRM. The user works with a card in Bitrix24, while the application interface appears inside the card: in a custom lead field or on a separate tab.

Two scenarios are available. The first shows how to add a field to a lead that opens the application interface. The second explains how to add a tab to a CRM card using a deal as an example.

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
|| **If necessary** | **Primary Method** | **Placement Code** | **Open** ||
|| Show the application interface inside the lead custom field | [userfieldtype.add](../../../api-reference/widgets/user-field/userfieldtype-add.md), [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) | `USERFIELD_TYPE` | [Embed a widget into a lead as a custom property](./widget-as-field-in-lead-page.md) ||
|| Add a tab to a CRM card using a deal as an example | [placement.bind](../../../api-reference/widgets/placement-bind.md) | `CRM_DEAL_DETAIL_TAB` | [Embed a widget into a CRM item tab](./widget-as-detail-tab.md) ||
|| Clarify tab codes and the data received by the handler | [placement.bind](../../../api-reference/widgets/placement-bind.md) | `CRM_XXX_DETAIL_TAB` | [Tab in CRM card CRM_XXX_DETAIL_TAB](../../../api-reference/widgets/crm/detail-tab.md) ||
|| Learn how custom CRM field types work | [userfieldtype.add](../../../api-reference/widgets/user-field/userfieldtype-add.md) | `USERFIELD_TYPE` | [Custom field types in CRM](../../../api-reference/crm/universal/user-defined-fields/userfield-type.md) ||
|#

## Continue Exploring

- [{#T}](../../../api-reference/widgets/index.md)
- [{#T}](../../../api-reference/widgets/crm/index.md)
- [{#T}](../../../api-reference/widgets/user-field/index.md)

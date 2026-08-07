# Widgets in CRM: Overview of Placements

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Placements add the application interface inside CRM: its own tab in an object card, an item in the list context menu, a button above the timeline, an item in the automation rules designer, or a report in CRM Analytics.

Placements fall into two groups. The first group works with a specific object type, and the placement code contains the name of that type: `CRM_DEAL_DETAIL_TAB`, `CRM_LEAD_LIST_MENU`. The second group belongs to the CRM section as a whole, and the code carries no object name: `CRM_ANALYTICS_MENU`, `CRM_FUNNELS_TOOLBAR`.

To register a widget, use the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter.

> Quick navigation: [all placements](#all-placements)

## How to Choose a Placement

Choose a placement by the task your application solves:

- add an action to an element in a list — [CRM_XXX_LIST_MENU](./list-menu.md)
- add an action to the whole list rather than to a single element — [CRM_XXX_LIST_TOOLBAR](./list-toolbar.md)
- add a separate screen with application data to a card — [CRM_XXX_DETAIL_TAB](./detail-tab.md)
- add a button next to the activities and comments of a card — [CRM_XXX_DETAIL_ACTIVITY](./detail-activity.md), and to build its interface with Bitrix24 tools — [additional placement features](./detail-activity-area.md)
- add an action for the whole card, next to tasks and documents — [CRM_XXX_DETAIL_TOOLBAR](./detail-toolbar.md)
- add an action to an individual activity record in the timeline — [CRM_XXX_ACTIVITY_TIMELINE_MENU](./activity-timeline-menu.md)
- generate a document for an object — [CRM_XXX_DOCUMENTGENERATOR_BUTTON](./document-generator-button.md)
- extend automation — [CRM_XXX_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md)
- extend pipelines and tunnels — [CRM_FUNNELS_TOOLBAR](./funnels-toolbar.md)
- show your own report — [CRM_ANALYTICS_MENU](./analytics-menu.md), and to add an action for the analytics section — [CRM_ANALYTICS_TOOLBAR](./analytics-toolbar.md)
- show a report next to the ready-made BI analytics reports — [BI_ANALYTICS_MENU](./bi-analytics-menu.md)
- search for a client in an external source and fill it into the form — [CRM_DETAIL_SEARCH](./detail-search.md)
- fill in company data from an external source — [details autofill](./requisites-autocomplete/index.md)

## How to Get Started

1. Choose a placement for your scenario and assemble the placement code: substitute the object type name if the code contains one. For custom object types, the numeric type identifier is substituted instead of the name — `CRM_DYNAMIC_183_DETAIL_TAB`.
2. Register the handler with the [placement.bind](../placement-bind.md) method and pass the code in the `PLACEMENT` parameter. The method is available to an administrator only and requires the application context: a placement cannot be bound with a webhook.
3. Complete the application installation. Until then, the widget is not displayed in the interface.
4. Open the place in the interface and call the widget. Where exactly the item is located is described on each placement page in the "Where to Find It in the Interface" section.
5. Parse `PLACEMENT_OPTIONS` in the handler — it carries the call context: the identifier of the object or activity, or the address of the page the widget was opened from.

## What the Handler Receives

Placements of the section pass the same set of standard parameters to the handler. The exception is [BI_ANALYTICS_MENU](./bi-analytics-menu.md): this placement opens the handler address with a regular GET request and passes nothing to it.

{% include notitle [description of standard data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context. The universal `URI` key arrives for every placement, while the set of the remaining keys is specific to each placement.

#|
|| **Placement** | **Own Keys** | **What Is Passed** ||
|| [CRM_XXX_LIST_MENU](./list-menu.md) | `ID` | Identifier of the element whose menu the widget is opened from ||
|| [CRM_XXX_LIST_TOOLBAR](./list-toolbar.md) | none | The widget opens above the list, not above an element ||
|| [CRM_XXX_DETAIL_TAB](./detail-tab.md) | `ID` | Identifier of the object whose card the widget is opened in ||
|| [CRM_XXX_DETAIL_ACTIVITY](./detail-activity.md) | `ID` | Identifier of the object whose timeline the widget is opened in ||
|| [CRM_XXX_DETAIL_TOOLBAR](./detail-toolbar.md) | `ID` or `ENTITY_ID` | Identifier of the object. The key name depends on the object type ||
|| [CRM_XXX_ACTIVITY_TIMELINE_MENU](./activity-timeline-menu.md) | `ENTITY_ID`, `ASSOCIATED_ENTITY_ID`, `ASSOCIATED_ENTITY_TYPE_ID` | Identifiers of the object and of the activity whose record the widget is opened on ||
|| [CRM_XXX_DOCUMENTGENERATOR_BUTTON](./document-generator-button.md) | `ENTITY_ID` | Identifier of the object the document is generated for ||
|| [CRM_XXX_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md) | none | The pipeline identifier can be taken from the path in `URI` ||
|| [CRM_FUNNELS_TOOLBAR](./funnels-toolbar.md) | none | The widget opens above the pipeline list ||
|| [CRM_ANALYTICS_MENU](./analytics-menu.md) | none | The widget opens in the analytics section ||
|| [CRM_ANALYTICS_TOOLBAR](./analytics-toolbar.md) | none | The widget opens in the analytics section ||
|| [CRM_DETAIL_SEARCH](./detail-search.md) | `entityTypeName`, `searchQuery` | Client type and the search query from the form ||
|| [BI_ANALYTICS_MENU](./bi-analytics-menu.md) | — | `PLACEMENT_OPTIONS` is not passed: the handler is opened with a GET request ||
|#

## Connection with Other Objects

**CRM object.** The identifier from `PLACEMENT_OPTIONS` indicates which element the handler was called for. Object data can be retrieved with the [crm.item.get](../../crm/universal/crm-item-get.md) method by passing the `entityTypeId` of the required [object type](../../crm/data-types.md#object_type), or with the method of its own section: [crm.deal.get](../../crm/deals/crm-deal-get.md), [crm.lead.get](../../crm/leads/crm-lead-get.md), [crm.contact.get](../../crm/contacts/crm-contact-get.md), [crm.company.get](../../crm/companies/crm-company-get.md), [crm.quote.get](../../crm/quote/crm-quote-get.md).

**Activity.** The `ASSOCIATED_ENTITY_ID` key indicates which activity record the widget is opened on. Activity data is returned by the [crm.activity.get](../../crm/timeline/activities/activity-base/crm-activity-get.md) method.

**Custom object type.** The type identifier does not arrive as a separate key. It can be taken from the `PLACEMENT` parameter value: for the `CRM_DYNAMIC_183_DETAIL_TAB` code, the type identifier is `183`.

**Call page.** The universal `URI` key contains the path of the Bitrix24 page the widget was opened from. The handler uses it to restore the scenario when the placement has no keys of its own.

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| `placement.bind` returns `Application context required` | Register the placement on behalf of an application. A placement cannot be bound with a webhook ||
|| The widget is registered but does not appear in the interface | Complete the [application installation](../../../settings/app-installation/installation-finish.md) and reload the page ||
|| The item cannot be found in a card or in a list | Some placements are displayed under *More* or in the *Bitrix24 Market* submenu when there are more items than fit in the row. The path is described on the placement page ||
|| The placement code is assembled for an object type that does not support this placement | Check the code against the section table: not all object types are supported in all placements ||
|| The handler does not find the object identifier in the request body | The identifier arrives inside `PLACEMENT_OPTIONS` as a separate JSON string, not as a separate parameter ||
|#

## Overview of Placements {#all-placements}

> Scope: [`placement, crm`](../../scopes/permissions.md)

The exception is `BI_ANALYTICS_MENU`: the placement is declared in the global [`placement`](../../scopes/permissions.md) scope and requires no separate CRM access.

#|
|| **Placement** | **When to Use** ||
|| [CRM_XXX_LIST_MENU](./list-menu.md) | Context menu item of an element in a list ||
|| [CRM_XXX_LIST_TOOLBAR](./list-toolbar.md) | Menu item above the list of elements ||
|| [CRM_XXX_DETAIL_TAB](./detail-tab.md) | Separate tab in an element card ||
|| [CRM_XXX_DETAIL_ACTIVITY](./detail-activity.md) | Button in the panel above the card timeline ||
|| [CRM_XXX_DETAIL_TOOLBAR](./detail-toolbar.md) | Top button menu item of a card ||
|| [CRM_XXX_ACTIVITY_TIMELINE_MENU](./activity-timeline-menu.md) | Context menu item of an activity record in the timeline ||
|| [CRM_XXX_DOCUMENTGENERATOR_BUTTON](./document-generator-button.md) | Dropdown menu item of the document generator ||
|| [CRM_XXX_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md) | Button in the automation rules designer ||
|| [CRM_FUNNELS_TOOLBAR](./funnels-toolbar.md) | Button in the sales pipelines and tunnels ||
|| [CRM_ANALYTICS_MENU](./analytics-menu.md) | Application report in the left menu of CRM Analytics ||
|| [CRM_ANALYTICS_TOOLBAR](./analytics-toolbar.md) | Button in the CRM Analytics header ||
|| [CRM_DETAIL_SEARCH](./detail-search.md) | Client search in an external source from the CRM form ||
|| [BI_ANALYTICS_MENU](./bi-analytics-menu.md) | Application report in the BI analytics menu ||
|| [CRM_REQUISITE_AUTOCOMPLETE, CRM_BANK_DETAIL_AUTOCOMPLETE](./requisites-autocomplete/index.md) | Filling in company data and bank details from an external source ||
|| [Additional features of CRM_XXX_DETAIL_ACTIVITY](./detail-activity-area.md) | Interface of the button above the timeline built with Bitrix24 tools ||
|#

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../placement-list.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../../settings/interactivity/index.md)

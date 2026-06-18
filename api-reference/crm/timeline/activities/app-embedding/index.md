# Embedding Applications into CRM Activities

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A CRM activity is a record of customer interaction: a call, a meeting, an e-mail, or a task. You can embed an application into CRM activities. An employee can trigger external service actions directly from the lead, deal, estimate, new invoice, or SPA card. There will be no need to open a separate program.

> Quick links: [Scenarios and Embedding Points](#all-placements)
>
> User documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Available Scenarios

An application can work with CRM activities in two ways:

**Action for an existing activity.** The application adds an item to the activity context menu. After selecting the item, Bitrix24 opens the application handler. There are two embedding points for this scenario: a menu item in the activity list and a menu item in the CRM card timeline. Menu items are connected via the [widget mechanism](../../../../widgets/index.md).

**Activity created by the application.** The application creates an activity using the [crm.activity.add](../activity-base/crm-activity-add.md) method with provider `REST_APP`. `REST_APP` is a fixed value for the `PROVIDER_ID` field, which indicates that the activity was created by the application. When such an activity is clicked, Bitrix24 opens the application.

To ensure the application opens inside Bitrix24, it requires its own web page — the handler. The page must be accessible via an external URL so that Bitrix24 can open it.

## Getting Started

1. Prepare a handler — an application page with a URL accessible from an external network.
2. Select a scenario and an embedding point in the [How to Choose a Scenario and an Embedding Point](#all-placements) table.

**If the application adds an item to the activity menu.** Register the handler using the [placement.bind](../../../../widgets/placement-bind.md) method and pass the required placement code to `PLACEMENT`. See the request example in the [Code Examples](../../../../widgets/placement-bind.md#code-examples) block of the [placement.bind](../../../../widgets/placement-bind.md) method. After registration, verify that the menu item appears in the CRM activity list or in the CRM card timeline.

**If the application creates its own activity in the CRM.** Use the [crm.activity.add](../activity-base/crm-activity-add.md) method within the installed application and pass the value `PROVIDER_ID=REST_APP` to `fields`. This allows Bitrix24 to recognize that the activity belongs to the application and, upon opening, display the application page in a slider.

Activities with provider `REST_APP` can only be created, updated, or deleted from within the application. If [crm.activity.add](../activity-base/crm-activity-add.md), [crm.activity.update](../activity-base/crm-activity-update.md), or [crm.activity.delete](../activity-base/crm-activity-delete.md) methods are called via a webhook, Bitrix24 will return error `Application context required`.

You can pass the application activity type in `PROVIDER_TYPE_ID`, for example, `LINK`. The type helps the application distinguish between different types of created activities. If the parameter is not passed, Bitrix24 uses `LINK` by default. After creation, open the activity and verify that Bitrix24 opens the application page in a slider.

## Connection with Other Objects

**CRM Activities.** Embedding works with an activity as a CRM object: a menu item is added to an existing activity, or the application creates its own activity. To work with activities, use the [crm.activity.add](../activity-base/crm-activity-add.md), [crm.activity.get](../activity-base/crm-activity-get.md), [crm.activity.update](../activity-base/crm-activity-update.md), and [crm.activity.delete](../activity-base/crm-activity-delete.md) methods.

**CRM Card.** Timeline embedding points link an application action to the CRM card where the activity is open.

## How to Select a Scenario and Embedding Point {#all-placements}

When Bitrix24 opens a handler, it passes the call data to the page in `PLACEMENT_OPTIONS`. This is a set of parameters through which the application understands where it was opened from and which activity it needs to work with. The composition `PLACEMENT_OPTIONS` depends on the selected embedding point.

The `CRM_XXX_ACTIVITY_TIMELINE_MENU` code in the table is a template for embedding points in the CRM card timeline. Instead of `XXX`, the card type is used. 
A complete list of codes can be found on the [CRM Activity Context Menu Item Page](../../../../widgets/crm/activity-timeline-menu.md).

#|
|| **If needed** | **Use** | **What the handler receives** ||
|| Add an item to the activity in the CRM activity list | [`CRM_ACTIVITY_LIST_MENU`](../../../../widgets/crm/index.md) | [`PLACEMENT_OPTIONS`](../../../../widgets/crm/index.md#placement_options) with activity ID `ID` ||
|| Add an item to the activity in the CRM card timeline | [`CRM_XXX_ACTIVITY_TIMELINE_MENU`](../../../../widgets/crm/activity-timeline-menu.md) | [`PLACEMENT_OPTIONS`](../../../../widgets/crm/activity-timeline-menu.md#placement_options) with CRM card ID `ENTITY_ID` and activity ID `ASSOCIATED_ENTITY_ID` ||
|| Create an activity that opens the application | [How to create an activity from the application](./activity-app.md) | [`PLACEMENT_OPTIONS`](./activity-app.md) with activity ID `activity_id` ||
|#

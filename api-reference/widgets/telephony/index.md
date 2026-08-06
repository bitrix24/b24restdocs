# Widgets in Telephony: Overview of Placements

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Placements of the section add the application interface to telephony: a tab in the call card during a conversation and a report in the call analytics menu. Both placements require the `telephony` scope.

To register a widget, use the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter.

An external WebRTC client is embedded differently. It has no placement code of its own: the client is loaded into the `PAGE_BACKGROUND_WORKER` placement, and telephony is connected with the methods of the [{#T}](../../telephony/index.md) section. The scenario is described on the [{#T}](./webrtc.md) page.

> Quick navigation: [all placements](#all-placements)

## How to Choose a Placement

Choose a placement by the task your application solves:

- show client data right during a conversation — [CALL_CARD](./call-card.md)
- add your own report to the built-in call analytics — [TELEPHONY_ANALYTICS_MENU](./analytics-menu.md)

The placements differ in the condition that invokes them. The `CALL_CARD` handler is invoked when the user opens the tab in the call card, and it receives the conversation context. This placement exists only during a call: without an active call there is no card and nothing to check the widget against.

The `TELEPHONY_ANALYTICS_MENU` handler is invoked when the user selects an item in the call statistics menu. The placement has no context of its own, but it is available at any moment.

## How to Get Started

1. Choose a placement for your scenario.
2. Register the handler with the [placement.bind](../placement-bind.md) method and pass the code in the `PLACEMENT` parameter. The method is available to an administrator only and requires the application context: a placement cannot be bound with a webhook.
3. Complete the application installation. Until then, the widget is not displayed in the interface.
4. Reload the Bitrix24 page. The lists of call card tabs and analytics menu items are built when the page loads.
5. Call the widget. For `TELEPHONY_ANALYTICS_MENU`, open the `/report/telephony/` page and select the application item. For `CALL_CARD`, you need a call: register an external call with the [telephony.externalCall.register](../../telephony/telephony-external-call-register.md) method and the `SHOW` = 1 parameter.
6. Parse `PLACEMENT_OPTIONS` in the handler — it carries the call context.

## What the Handler Receives

Both placements pass the same set of standard parameters to the handler.

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context. The universal `URI` key arrives for both placements, while own keys exist only for the call card.

#|
|| **Placement** | **Own Keys** | **What Is Passed** ||
|| [CALL_CARD](./call-card.md) | `CALL_ID`, `PHONE_NUMBER`, `LINE_NUMBER`, `LINE_NAME`, `CRM_ENTITY_TYPE`, `CRM_ENTITY_ID`, `CRM_BINDINGS`, `CRM_ACTIVITY_ID`, `CALL_DIRECTION`, `CALL_STATE`, `CALL_LIST_MODE` | The call context: call identifier, client and line numbers, conversation direction and state, linked CRM items ||
|| [TELEPHONY_ANALYTICS_MENU](./analytics-menu.md) | none | Only the address of the analytics page in `URI` ||
|#

Neither placement of the section supports the `OPTIONS` parameter of the [placement.bind](../placement-bind.md) method: the values passed are not retained, and [placement.get](../placement-get.md) returns an empty array.

## Relationship With Other Objects

**Call.** The `CALL_ID` identifier from the `CALL_CARD` context is the same one returned by [telephony.externalCall.register](../../telephony/telephony-external-call-register.md). The handler uses it to finish the call with the [telephony.externalCall.finish](../../telephony/telephony-external-call-finish.md) method and to attach a call recording.

**CRM items.** The `CRM_ENTITY_TYPE` and `CRM_ENTITY_ID` keys point to the item the call is linked to, and `CRM_BINDINGS` points to all linked items. The data is returned by the methods of the corresponding CRM sections.

**CRM activity.** The `CRM_ACTIVITY_ID` identifier points to the activity created for the call. The activity data is returned by the [crm.activity.get](../../crm/timeline/activities/activity-base/crm-activity-get.md) method.

**Phone lines.** The line name in `LINE_NAME` refers to a line added with the [telephony.externalLine.add](../../telephony/telephony-external-line-add.md) method.

## Common Mistakes

#|
|| **Mistake** | **How to Resolve** ||
|| `placement.bind` returns `Application context required` | Register the placement on behalf of the application. A placement cannot be bound with a webhook ||
|| The widget is registered but does not appear in the interface | Complete the [application installation](../../../settings/app-installation/installation-finish.md) and reload the Bitrix24 page ||
|| The `CALL_CARD` tab does not appear in the call card | The list of tabs is built when the page loads. Reload the page and raise the call again ||
|| The handler does not find the activity identifier | If no activity was created for the call, `CRM_ACTIVITY_ID` carries the string `undefined`. Check the value before using it ||
|| The handler does not recognize `CRM_ENTITY_TYPE` | The key carries the symbolic code of the type — `LEAD`, `DEAL`, `CONTACT`, `COMPANY` — not the numeric identifier ||
|| The widget does not see the second linked CRM item | The `CRM_ENTITY_TYPE` and `CRM_ENTITY_ID` keys name the primary item only. The full list of links arrives in `CRM_BINDINGS` ||
|#

## Overview of Placements {#all-placements}

> Scope: [`placement, telephony`](../../scopes/permissions.md)

#|
|| **Placement** | **When to Use** ||
|| [CALL_CARD](./call-card.md) | Tab in the call card ||
|| [TELEPHONY_ANALYTICS_MENU](./analytics-menu.md) | Menu item in call analytics ||
|#

## Continue Your Exploration

- [{#T}](./webrtc.md)
- [{#T}](../placement-bind.md)
- [{#T}](../placement-list.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../telephony/index.md)
- [{#T}](../../../settings/interactivity/index.md)

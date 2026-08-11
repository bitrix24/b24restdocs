# Placement Catalog

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A placement is a location in the Bitrix24 interface where an application adds its own interface: a tab in a card, a menu item, a button in a panel, or a separate page. When the user opens a widget, Bitrix24 opens the address of the application handler and passes the call data to it — the application authorization and the context of the location the widget was called from.

The handler is registered with the [placement.bind](./placement-bind.md) method: the code of the required location is passed in the `PLACEMENT` parameter. One application can register several widgets, including several in the same location — for example, two tabs in a deal card or two context menu items in the task list. [Universal placements](./universal/index.md) are limited to a single handler.

This page is a complete catalog of placements. The registration procedure, permissions, handler data, and common mistakes are described in the [Widget Integration Mechanism](./index.md) article.

> Quick navigation: [all placements](#all-placements)

## How to Choose a Placement

Placements with the same meaning exist in different Bitrix24 tools, so choose a placement by task rather than by section:

- show a screen of your own inside an entity card — `CRM_XXX_DETAIL_TAB`, `TASK_VIEW_TAB`, `TASK_VIEW_SIDEBAR`, `SONET_GROUP_DETAIL_TAB`, `CALL_CARD`
- add an action to a single element in a list — `CRM_XXX_LIST_MENU`, `TASK_LIST_CONTEXT_MENU`
- add an action to the whole list — `CRM_XXX_LIST_TOOLBAR`, `TASK_USER_LIST_TOOLBAR`, `TASK_GROUP_LIST_TOOLBAR`, `SONET_GROUP_TOOLBAR`, `CRM_FUNNELS_TOOLBAR`
- add a button to a card panel — `CRM_XXX_DETAIL_TOOLBAR`, `CRM_XXX_DETAIL_ACTIVITY`, `CRM_XXX_DOCUMENTGENERATOR_BUTTON`, `TASK_VIEW_TOP_PANEL`
- take over the entire working area for a section or a report of your own — `LEFT_MENU`, `IM_NAVIGATION`, `CRM_ANALYTICS_MENU`, `BI_ANALYTICS_MENU`, `TELEPHONY_ANALYTICS_MENU`
- work with the current conversation — `IM_TEXTAREA`, `IM_SIDEBAR`, `IM_CONTEXT_MENU`, `IMMOBILE_CONTEXT_MENU`
- extend automation — `CRM_XXX_ROBOT_DESIGNER_TOOLBAR`, `TASK_ROBOT_DESIGNER_TOOLBAR`, `SONET_GROUP_ROBOT_DESIGNER_TOOLBAR`
- fill a card with data from an external source — `CRM_DETAIL_SEARCH`, `CRM_REQUISITE_AUTOCOMPLETE`, `CRM_BANK_DETAIL_AUTOCOMPLETE`
- connect a communication channel of your own — `CONTACT_CENTER`, `SETTING_CONNECTOR`
- add a view of your own — `CALENDAR_GRIDVIEW`
- extend the user menu or the employee card — `USER_PROFILE_MENU`, `USER_PROFILE_TOOLBAR`
- work without a visible interface element — `PAGE_BACKGROUND_WORKER`, `REST_APP_URI`

## How to Build the Placement Code

Some codes contain the name of the entity type. Substitute the required value for `XXX`.

#|
|| **Code Template** | **What to Substitute** ||
|| `CRM_XXX_...` | `LEAD`, `DEAL`, `CONTACT`, `COMPANY`, `QUOTE`, `SMART_INVOICE`, `ORDER`, `ACTIVITY`. For a [custom entity type](../crm/universal/index.md), use `DYNAMIC_` and the numeric identifier of the type: `CRM_DYNAMIC_183_DETAIL_TAB` ||
|| `TASK_XXX_LIST_TOOLBAR` | `USER` for the task list of a user, `GROUP` for the task list of a project ||
|#

The set of supported types differs from placement to placement. The automation rules designer works only with leads, deals, new invoices, and custom entity types, while a tab in a card also supports an online store order. The exact list of codes is given on the page of each placement.

The actual list of locations available to an application on a specific Bitrix24 is returned by the [placement.list](./placement-list.md) method. Verify the code with a call rather than by guesswork.

## Embeddings Connected Differently

Some embeddings are registered with a method other than `placement.bind`.

#|
|| **What to Embed** | **How It Is Connected** ||
|| An item in the main menu that opens the main page of the application | Application configurations, no REST API calls. Where to specify them is described in the [Widget Integration Mechanism](./index.md) article. The [LEFT_MENU](./left-menu.md) placement is needed for the item to open a registered handler instead of the main address of the application ||
|| [Connector settings page](./setting-connector.md) | The `PLACEMENT_HANDLER` parameter of the [imconnector.register](../imopenlines/imconnector/imconnector-register.md) method. Bitrix24 creates the binding itself when it registers the connector ||
|| [Custom field type in a CRM card](./user-field/index.md) | The [userfieldtype.add](./user-field/userfieldtype-add.md) method. The application registers not a field but its type, and specifies the address of the handler that opens where the field is displayed ||
|| [WebRTC scenario](./telephony/webrtc.md) | There is no separate placement: the scenario runs on top of [PAGE_BACKGROUND_WORKER](./universal/background-worker.md) ||
|#

## All Placements {#all-placements}

The handler is registered with the `placement.bind` method, so an application always needs the [`placement`](../scopes/permissions.md) scope. The column shows the second scope of the pair given in the header of the placement page. If the column says not required, the placement is declared in the global `placement` scope and needs no second scope.

The exception is `SETTING_CONNECTOR`: it is connected with the `imconnector.register` method and requires only the `imopenlines` scope. For messenger placements, the `im` scope is needed not for registration but by the handler — to work with the chat by the received `dialogId`.

#|
|| **Code** | **Where It Appears in the Interface** | **Additional Scope** ||
|| [BI_ANALYTICS_MENU](./crm/bi-analytics-menu.md) | Item in the BI analytics menu. The only placement whose handler is opened with a GET request without call data | not required ||
|| [CALENDAR_GRIDVIEW](./calendar.md) | A view of your own in the calendar | `calendar` ||
|| [CALL_CARD](./telephony/call-card.md) | Tab in the call card | `telephony` ||
|| [CONTACT_CENTER](./contact-center.md) | Tile in the Contact Center | `contact_center` ||
|| [CRM_ANALYTICS_MENU](./crm/analytics-menu.md) | Application report in the left menu of CRM Analytics | `crm` ||
|| [CRM_ANALYTICS_TOOLBAR](./crm/analytics-toolbar.md) | Button in the CRM Analytics header | `crm` ||
|| [CRM_BANK_DETAIL_AUTOCOMPLETE](./crm/requisites-autocomplete/bank-detail-autocomplete.md) | Search source in the bank details field of a CRM card | `crm` ||
|| [CRM_DETAIL_SEARCH](./crm/detail-search.md) | Client search source in a CRM card | `crm` ||
|| [CRM_FUNNELS_TOOLBAR](./crm/funnels-toolbar.md) | Button in the sales pipelines and tunnels | `crm` ||
|| [CRM_REQUISITE_AUTOCOMPLETE](./crm/requisites-autocomplete/requisite-autocomplete.md) | Search source in the details field of a CRM card | `crm` ||
|| [CRM_XXX_ACTIVITY_TIMELINE_MENU](./crm/activity-timeline-menu.md) | Context menu item of an activity record in the card timeline | `crm` ||
|| [CRM_XXX_DETAIL_ACTIVITY](./crm/detail-activity.md) | Button in the panel above the card timeline | `crm` ||
|| [CRM_XXX_DETAIL_TAB](./crm/detail-tab.md) | Tab in a CRM element card | `crm` ||
|| [CRM_XXX_DETAIL_TOOLBAR](./crm/detail-toolbar.md) | Top button menu item of a card | `crm` ||
|| [CRM_XXX_DOCUMENTGENERATOR_BUTTON](./crm/document-generator-button.md) | Document generator menu item in a card | `crm` ||
|| [CRM_XXX_LIST_MENU](./crm/list-menu.md) | Context menu item of an element in a list | `crm` ||
|| [CRM_XXX_LIST_TOOLBAR](./crm/list-toolbar.md) | Menu item above the list of elements | `crm` ||
|| [CRM_XXX_ROBOT_DESIGNER_TOOLBAR](./crm/robot-designer-toolbar.md) | Button in the CRM automation rules designer | `crm` ||
|| [IM_CONTEXT_MENU](./im/context-menu.md) | Item in the context menu of a message | `im` ||
|| [IM_NAVIGATION](./im/navigation.md) | Item in the messenger navigation menu | `im` ||
|| [IM_SIDEBAR](./im/sidebar.md) | Item in the chat sidebar | `im` ||
|| [IM_SMILES_SELECTOR](./im/smile-selector.md) | Archived placement for the emoji collection. Do not use for new integrations | not required ||
|| [IM_TEXTAREA](./im/textarea.md) | Item in the panel above the chat input field | `im` ||
|| [IMMOBILE_CONTEXT_MENU](./mobile-app.md) | Item in the *Apps* menu in a chat of the mobile application | `im` ||
|| [LEFT_MENU](./left-menu.md) | Item in the main menu of Bitrix24 | not required ||
|| [PAGE_BACKGROUND_WORKER](./universal/background-worker.md) | Background handler on all pages, without a visible interface element | not required ||
|| [REST_APP_URI](./universal/app-url.md) | Opening the application in a slider through a link in a message, comment, or task | not required ||
|| [SETTING_CONNECTOR](./setting-connector.md) | Open channel connector settings page. The only placement that is connected with the `imconnector.register` method instead of `placement.bind` | `imopenlines` only ||
|| [SONET_GROUP_DETAIL_TAB](./workgroups/detail-tab.md) | Menu item of a workgroup or project | `sonet_group` ||
|| [SONET_GROUP_ROBOT_DESIGNER_TOOLBAR](./workgroups/robot-designer-toolbar.md) | Button in the workgroup automation rules designer | `sonet_group` ||
|| [SONET_GROUP_TOOLBAR](./workgroups/toolbar.md) | Extensions menu item of a workgroup or project | `sonet_group` ||
|| [TASK_GROUP_LIST_TOOLBAR](./task/list-toolbar.md) | Menu item above the task list of a project | `task` ||
|| [TASK_LIST_CONTEXT_MENU](./task/list-context-menu.md) | Context menu item of a task in the list | `task` ||
|| [TASK_ROBOT_DESIGNER_TOOLBAR](./task/robot-designer-toolbar.md) | Button in the task automation rules designer | `task` ||
|| [TASK_USER_LIST_TOOLBAR](./task/list-toolbar.md) | Menu item above the task list of a user | `task` ||
|| [TASK_VIEW_SIDEBAR](./task/view-sidebar.md) | Widget in the task card, formerly the right panel | `task` ||
|| [TASK_VIEW_TAB](./task/view-tab.md) | Widget in the task card, formerly a tab | `task` ||
|| [TASK_VIEW_TOP_PANEL](./task/view-top-panel.md) | Widget in the task card, formerly a button in the top panel | `task` ||
|| [TELEPHONY_ANALYTICS_MENU](./telephony/analytics-menu.md) | Menu item in call analytics | `telephony` ||
|| [USER_PROFILE_MENU](./user-profile/profile-menu.md) | An item in the user menu, available from any page | `user` ||
|| [USER_PROFILE_TOOLBAR](./user-profile/profile-toolbar.md) | An item in the employee card menu | `user` ||
|#

## Section Overviews

A section overview breaks the placements down by scenario and describes the general workflow and the call context:

- [Widgets in CRM](./crm/index.md) — element cards and lists, analytics, automation, [autofilling details](./crm/requisites-autocomplete/index.md)
- [Widgets in Tasks](./task/index.md) — the task card, task lists, the automation rules designer
- [Widgets in Workgroups and Projects](./workgroups/index.md) — the workgroup menu and the workgroup automation rules designer
- [Chat Widgets](./im/index.md) — the chat, a message, the messenger navigation
- [Widgets in Telephony](./telephony/index.md) — the call card and call analytics
- [Widgets in User Profile](./user-profile/index.md) — the user menu and the employee card
- [Universal Widgets](./universal/index.md) — work with no attachment to a section: through a link and in the background
- [Custom Field Types](./user-field/index.md) — an interface of your own for displaying and editing a field in a CRM card

Standalone placements have no sections of their own: the [main menu](./left-menu.md), the [calendar](./calendar.md), the [Contact Center](./contact-center.md), the [connector settings](./setting-connector.md), and the [mobile application chat](./mobile-app.md).

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./placement-bind.md)
- [{#T}](./placement-get.md)
- [{#T}](./placement-list.md)
- [{#T}](./placement-unbind.md)
- [{#T}](./ui-interaction/index.md)
- [{#T}](./bx24-widget-methods.md)

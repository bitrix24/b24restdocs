# Widgets in Workgroups and Projects: Overview of Placements

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Placements add the application interface inside a workgroup or project: an item in the group menu, an item in the extensions menu, or a button in the automation rules designer.

All three placements of the section work in the context of a single group and require the `sonet_group` scope. [A project is a group with extended capabilities](../../sonet-group/index.md), so the placements are displayed both in groups and in projects.

To register a widget, use the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter.

> Quick navigation: [all placements](#all-placements)

## How to Choose a Placement

Choose a placement by the task your application solves:

- add a separate screen with application data to a group — [SONET_GROUP_DETAIL_TAB](./detail-tab.md)
- add an item to the extensions menu of a group — [SONET_GROUP_TOOLBAR](./toolbar.md)
- extend the automation of group tasks — [SONET_GROUP_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md)

The rendering location depends on the interface version. The classic view currently runs in most Bitrix24 accounts, while the new Projects AI view is being rolled out gradually. Projects AI has no extensions menu, so the `SONET_GROUP_TOOLBAR` item will not appear there. If your application needs an item in the group in both interface versions, use [SONET_GROUP_DETAIL_TAB](./detail-tab.md).

## How to Get Started

1. Choose a placement for your scenario.
2. Register the handler with the [placement.bind](../placement-bind.md) method and pass the code in the `PLACEMENT` parameter. The method is available to an administrator only and requires the application context: a placement cannot be bound with a webhook.
3. Complete the application installation. Until then, the widget is not displayed in the interface.
4. Open the workgroup and call the widget. Where exactly the item is located is described on each placement page in the "Where to Find It in the Interface" section.
5. Parse `PLACEMENT_OPTIONS` in the handler — it carries the call context: the workgroup identifier or the address of the page the widget was opened from.

## What the Handler Receives

All placements of the section pass the same set of standard parameters to the handler.

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context. The universal `URI` key arrives for every placement, while the set of the remaining keys is specific to each placement.

#|
|| **Placement** | **Own Keys** | **What Is Passed** ||
|| [SONET_GROUP_DETAIL_TAB](./detail-tab.md) | `GROUP_ID` | Identifier of the group whose menu the widget is opened from ||
|| [SONET_GROUP_TOOLBAR](./toolbar.md) | none | The group identifier can be taken from the path in `URI` ||
|| [SONET_GROUP_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md) | `GROUP_ID` | Identifier of the group whose automation the user is configuring ||
|#

## Connection with Other Objects

**Workgroup.** The `GROUP_ID` identifier indicates which group the handler was called for. Workgroup data is returned by the [sonet_group.get](../../sonet-group/sonet-group-get.md) method.

**Group tasks.** The `SONET_GROUP_ROBOT_DESIGNER_TOOLBAR` placement is called from the task automation settings. Data of the tasks themselves is returned by the [methods of the tasks section](../../tasks/index.md).

**Call page.** The universal `URI` key contains the path of the Bitrix24 page the widget was opened from. For `SONET_GROUP_TOOLBAR` it is the only source of the group identifier: in the path `/workgroups/group/10/tasks/` the identifier is `10`.

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| `placement.bind` returns `Application context required` | Register the placement on behalf of an application. A placement cannot be bound with a webhook ||
|| The widget is registered but does not appear in the interface | Complete the [application installation](../../../settings/app-installation/installation-finish.md) and reload the page ||
|| The `SONET_GROUP_TOOLBAR` item does not appear in the group | The Projects AI interface has no extensions menu, and the placement has no rendering location there. Use [SONET_GROUP_DETAIL_TAB](./detail-tab.md) ||
|| The handler does not find `GROUP_ID` | `SONET_GROUP_TOOLBAR` has no keys of its own. Take the identifier from the path in `URI` ||
|| The item does not appear in the group tasks | Check the code: the menu and the automation rules in the tasks section are handled by [TASK_GROUP_LIST_TOOLBAR](../task/list-toolbar.md) and [TASK_ROBOT_DESIGNER_TOOLBAR](../task/robot-designer-toolbar.md), which require the `task` scope ||
|#

## Overview of Placements {#all-placements}

> Scope: [`placement, sonet_group`](../../scopes/permissions.md)

#|
|| **Placement** | **When to Use** ||
|| [SONET_GROUP_DETAIL_TAB](./detail-tab.md) | Menu item of a workgroup or project ||
|| [SONET_GROUP_TOOLBAR](./toolbar.md) | Extensions menu item of a workgroup or project ||
|| [SONET_GROUP_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md) | Button in the workgroup automation rules designer ||
|#

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../placement-list.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../../settings/interactivity/index.md)

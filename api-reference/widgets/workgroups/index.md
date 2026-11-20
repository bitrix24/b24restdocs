# Widgets in Workgroups and Projects: Overview of Placements

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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

The rendering location depends on the interface version. The classic view currently runs in most Bitrix24 accounts, while the new Projects AI view is being rolled out gradually. Projects AI has no extensions menu: the `SONET_GROUP_TOOLBAR` and `SONET_GROUP_DETAIL_TAB` items are displayed in the list of project features.

## How to Get Started

1. Choose a placement for your scenario.
2. Register the handler with the [placement.bind](../placement-bind.md) method and pass the code in the `PLACEMENT` parameter. The method requires the application context: a placement cannot be bound with a webhook. The handlers registered by the application are returned by the [placement.get](../placement-get.md) method.
3. Complete the application installation. Until then, the widget is not displayed in the interface.
4. Open the workgroup and call the widget. Where exactly the item is located is described on each placement page in the "Where to Find It in the Interface" section.
5. Parse `PLACEMENT_OPTIONS` in the handler — it carries the call context: the workgroup identifier or the address of the page the widget was opened from.

## What the Handler Receives

All placements of the section pass the same set of standard parameters to the handler.

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context. The universal `URI` key arrives for every placement if the browser passed the address of the page the widget is opened from. The set of the remaining keys is specific to each placement.

#|
|| **Placement** | **Own Keys** | **What Is Passed** ||
|| [SONET_GROUP_DETAIL_TAB](./detail-tab.md) | `GROUP_ID` | Identifier of the group whose menu the widget is opened from ||
|| [SONET_GROUP_TOOLBAR](./toolbar.md) | none | The group identifier can be taken from the path in `URI` ||
|| [SONET_GROUP_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md) | `GROUP_ID` | Identifier of the group whose automation the user is configuring ||
|#

This is how the `PLACEMENT_OPTIONS` value looks in the POST request to the handler. Values arrive as strings, including `GROUP_ID`.

`SONET_GROUP_DETAIL_TAB` — the item is opened from the menu of the group with the identifier `10`:

```json
{"GROUP_ID":"10","URI":"\/workgroups\/group\/10\/"}
```

`SONET_GROUP_TOOLBAR` — the item is opened from the extensions menu on the tasks page of the same group:

```json
{"URI":"\/workgroups\/group\/10\/tasks\/"}
```

Before use, convert the `PLACEMENT_OPTIONS` string from JSON, for example with the `json_decode` function in PHP or `JSON.parse` in JavaScript.

## Connection with Other Objects

**Workgroup.** The `GROUP_ID` identifier indicates which group the handler was called for. Workgroup data is returned by the [sonet_group.get](../../sonet-group/sonet-group-get.md) method.

**Group tasks.** The `SONET_GROUP_ROBOT_DESIGNER_TOOLBAR` placement is called from the task automation settings. Data of the tasks themselves is returned by the [methods of the tasks section](../../tasks/index.md).

**Call page.** The `URI` key contains the path of the Bitrix24 page the widget was opened from.

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| `placement.bind` returns `Application context required` | Register the placement on behalf of an application. A placement cannot be bound with a webhook ||
|| The widget is registered but does not appear in the interface | Complete the [application installation](../../../settings/app-installation/installation-finish.md) and reload the page ||
|| The `SONET_GROUP_TOOLBAR` item does not appear in the group | The Projects AI interface has no extensions menu. Look for the item in the list of project features ||
|| The `SONET_GROUP_TOOLBAR` handler does not find `GROUP_ID` | The placement does not pass `GROUP_ID`. Take the identifier from the path in `URI`. If `URI` did not arrive, retrieve the user's groups with the [sonet_group.user.groups](../../sonet-group/sonet-group-user-groups.md) method and offer to select the required one ||
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
- [{#T}](../placement-get.md)
- [{#T}](../placement-list.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../../settings/interactivity/index.md)

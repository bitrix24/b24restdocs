# Widgets in Tasks: Overview of Placements

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Placements add the application interface to tasks: an item in the context menu, an item in the dropdown menu above the list, a button in the automation rules designer, and your own screen inside the card.

To register a widget, use the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter.

> Quick navigation: [all placements](#all-placements)

## How to Choose a Placement

Choose a placement by the task your application solves:

- add an action for an individual task from the list — [TASK_LIST_CONTEXT_MENU](./list-context-menu.md)
- add an action for the entire task list — [TASK_USER_LIST_TOOLBAR and TASK_GROUP_LIST_TOOLBAR](./list-toolbar.md)
- add your own button to the task automation rules designer — [TASK_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md)
- add your own screen to the task card — [TASK_VIEW_TAB](./view-tab.md), [TASK_VIEW_SIDEBAR](./view-sidebar.md), or [TASK_VIEW_TOP_PANEL](./view-top-panel.md)

{% note info "" %}

Starting with module version `tasks` 25.700.0, tasks open in the [new card](../../tasks/tasks-new.md). The three card placements no longer have a place of their own in it: all three are rendered as identical rows in the *Applications* block and receive the same `taskId`. One placement out of the three is enough for a new integration: every registered placement adds one more row.

{% endnote %}

Placements in the menu of the workgroup or project itself are described in the workgroups section: [SONET_GROUP_DETAIL_TAB](../workgroups/detail-tab.md) and [SONET_GROUP_TOOLBAR](../workgroups/toolbar.md) require the `sonet_group` scope and are called from the group menu, not from tasks. The same section describes the [SONET_GROUP_ROBOT_DESIGNER_TOOLBAR](../workgroups/robot-designer-toolbar.md) placement: it is rendered in the same automation rules designer as `TASK_ROBOT_DESIGNER_TOOLBAR`, but only when the designer is opened from a project.

## How to Get Started

1. Choose a placement for your scenario. The [placement.list](../placement-list.md) method returns the codes available to the application in a specific Bitrix24.
2. Register the handler with the [placement.bind](../placement-bind.md) method and pass the code in the `PLACEMENT` parameter. On successful registration, the method returns `result: true` — the response breakdown and the error codes are on its page.
3. Limit the output to specific projects if you need to. The `groupId` parameter is supported only by `TASK_VIEW_TAB` and `TASK_VIEW_SIDEBAR` — see [OPTIONS at registration](#options).
4. Complete the application installation. Until then, the widget is not displayed in the interface.
5. Open the place in the interface and call the widget. Where exactly the item is located is described on each placement page in the "Where to Find It in the Interface" section.
6. Parse `PLACEMENT_OPTIONS` in the handler — it carries the call context: the identifier of a task, a user, or a project, as well as the address of the page the widget was opened from.

## What the Handler Receives

Bitrix24 passes the same set of standard parameters to every placement of the section. Only the call context in `PLACEMENT_OPTIONS` differs.

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 0063a02ba25315469678f946ece50010
    [AUTH_ID] => 9c52ba6600705a0700005a4b00000001f0f107e81691773d119eb941ad045e36
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 8cd1e16600705a0700005a4b00000001f0f1070aef2cbe270a6f27bcaf791e45
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => 3f0a7c19e5b84d2196c8ad470e5f2b31
    [APPLICATION_SCOPE] => task,placement
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => TASK_VIEW_TAB
    [PLACEMENT_OPTIONS] => {"taskId":"31","URI":"\/company\/personal\/user\/1\/tasks\/task\/view\/31\/"}
)

```

After parsing, the `PLACEMENT_OPTIONS` string from this example looks like this:

```json
{
    "taskId": "31",
    "URI": "/company/personal/user/1/tasks/task/view/31/"
}
```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context. The universal `URI` key arrives for every placement, while the set of the remaining keys is specific to each placement.

#|
|| **Placement** | **Own Keys** | **What Is Passed** ||
|| [TASK_LIST_CONTEXT_MENU](./list-context-menu.md) | `ID` | Identifier of the task whose menu the widget is opened from ||
|| [TASK_USER_LIST_TOOLBAR](./list-toolbar.md) | `USER_ID` | Identifier of the user whose task list the widget is opened above ||
|| [TASK_GROUP_LIST_TOOLBAR](./list-toolbar.md) | `GROUP_ID` | Identifier of the group or project whose task list the widget is opened above ||
|| [TASK_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md) | `USER_ID` or `GROUP_ID` | Automation context: the personal planner of a user or a project ||
|| [TASK_VIEW_TAB](./view-tab.md) | `taskId` | Identifier of the task whose card the widget is opened from ||
|| [TASK_VIEW_SIDEBAR](./view-sidebar.md) | `taskId` | Identifier of the task whose card the widget is opened from ||
|| [TASK_VIEW_TOP_PANEL](./view-top-panel.md) | `taskId` | Identifier of the task whose card the widget is opened from ||
|#

## OPTIONS at Registration via placement.bind {#options}

Connection parameters are passed in `OPTIONS` of the [placement.bind](../placement-bind.md) method. This is not the data that Bitrix24 passes to the handler when the placement is called. In the tasks section, connection parameters are supported by one pair of placements only.

#|
|| **Placement** | **`OPTIONS` Keys** ||
|| [TASK_VIEW_TAB](./view-tab.md#options), [TASK_VIEW_SIDEBAR](./view-sidebar.md#options) | `groupId` — comma-separated project identifiers, for example `11,12`. An empty value or a missing parameter means the widget is displayed in all tasks ||
|| Other placements of the section | `groupId` is not declared ||
|#

## Connection with Other Objects

**Task.** The `ID` key in the context menu of the list and the `taskId` key in the card indicate which task the handler was called for. Task data is returned by the [tasks.task.get](../../tasks/tasks-task-get.md) method.

**User.** The `USER_ID` key indicates whose task list or whose personal planner is open. User data is returned by the [user.get](../../user/user-get.md) method.

**Workgroup and project.** The `GROUP_ID` key indicates which group the task list or the automation belongs to. Group data is returned by the [sonet_group.get](../../sonet-group/sonet-group-get.md) method.

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| `placement.bind` returns `WRONG_AUTH_TYPE` with the description `Application context required` | Register the placement on behalf of an application. A placement cannot be bound with a webhook ||
|| The widget is registered but does not appear in the interface | Complete the [application installation](../../../settings/app-installation/installation-finish.md) and reload the page ||
|| `placement.bind` returns `ERROR_PLACEMENT_NOT_FOUND` | Check the code against the [Overview of Placements](#all-placements) table and make sure the application has been granted the `task` scope ||
|| `placement.bind` returns `ERROR_ARGUMENT` | Check the required parameters and their types. The name of the invalid parameter comes in the `argument` field ||
|| The handler does not find the task identifier | Read the identifier from the `ID` key in the context menu of the list and from `taskId` in the task card ||
|#

Other registration error codes are listed in the "Possible Error Codes" section of the [placement.bind](../placement-bind.md) page.

## Overview of Placements {#all-placements}

> Scope: [`placement, task`](../../scopes/permissions.md)

#|
|| **Placement** | **When to Use** ||
|| [TASK_LIST_CONTEXT_MENU](./list-context-menu.md) | Context menu item of a task in the list ||
|| [TASK_USER_LIST_TOOLBAR, TASK_GROUP_LIST_TOOLBAR](./list-toolbar.md) | Dropdown menu item above the task list of a user or a group ||
|| [TASK_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md) | Button in the task automation rules designer ||
|| [TASK_VIEW_TAB](./view-tab.md) | Your own screen inside a task, formerly a tab of the card ||
|| [TASK_VIEW_SIDEBAR](./view-sidebar.md) | Your own screen inside a task, formerly the right panel of the card ||
|| [TASK_VIEW_TOP_PANEL](./view-top-panel.md) | Your own screen inside a task, formerly a button in the top panel of the card ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../placements.md)
- [{#T}](../placement-bind.md)
- [{#T}](../placement-get.md)
- [{#T}](../placement-list.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../tasks/index.md)
- [{#T}](../../../settings/interactivity/index.md)

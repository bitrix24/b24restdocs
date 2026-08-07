# Widgets in Tasks: Overview of Placements

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Placements add the application interface to tasks: an item in the context menu of a task, an item in the dropdown menu above the list, a button in the automation rules designer, and a widget inside the task card.

All placements of the section require the `task` scope. The handler receives the call context: the identifier of a task, a user, or a project, depending on where the widget is opened from.

To register a widget, use the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter.

> Quick navigation: [all placements](#all-placements)

## How to Choose a Placement

Choose a placement by the task your application solves:

- add an action to an individual task in the list — [TASK_LIST_CONTEXT_MENU](./list-context-menu.md)
- add an action to the whole task list — [TASK_USER_LIST_TOOLBAR and TASK_GROUP_LIST_TOOLBAR](./list-toolbar.md)
- extend task automation — [TASK_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md)
- add your own screen to the task card — [TASK_VIEW_TAB](./view-tab.md), [TASK_VIEW_SIDEBAR](./view-sidebar.md), or [TASK_VIEW_TOP_PANEL](./view-top-panel.md)

The three placements of the card used to differ in rendering location: a tab, the right panel, and a button in the top panel. Starting with the `tasks 25.700.0` module version, the [new task card](../../tasks/tasks-new.md) was released, and none of them has a place of its own anymore — all three are rendered as identical rows in the "Applications" block. Previously registered widgets keep working, and one placement out of the three is enough for a new integration.

The menu of the workgroup or project itself belongs to another section: the [SONET_GROUP_DETAIL_TAB and SONET_GROUP_TOOLBAR](../workgroups/index.md) placements require the `sonet_group` scope and are called from the group menu, not from tasks. The [SONET_GROUP_ROBOT_DESIGNER_TOOLBAR](../workgroups/robot-designer-toolbar.md) placement is rendered in the same automation rules designer as `TASK_ROBOT_DESIGNER_TOOLBAR` but belongs to the workgroups section.

## How to Get Started

1. Choose a placement for your scenario.
2. Register the handler with the [placement.bind](../placement-bind.md) method and pass the code in the `PLACEMENT` parameter. The method is available to an administrator only and requires the application context: a placement cannot be bound with a webhook.
3. Limit the widget to specific projects if you need to. The `groupId` connection parameter is supported only by the [TASK_VIEW_TAB](./view-tab.md) and [TASK_VIEW_SIDEBAR](./view-sidebar.md) placements.
4. Complete the application installation. Until then, the widget is not displayed in the interface.
5. Open the place in the interface and call the widget. Where exactly the item is located is described on each placement page in the "Where to Find It in the Interface" section.
6. Parse `PLACEMENT_OPTIONS` in the handler — it carries the call context: the identifier of a task, a user, or a project, and the address of the page the widget was opened from.

## What the Handler Receives

All placements of the section pass the same set of standard parameters to the handler.

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

The key that carries the task identifier is named differently across the section: `ID` in the context menu of the list and `taskId` in the card.

## Connection with Other Objects

**Task.** The identifier from `PLACEMENT_OPTIONS` indicates which task the handler was called for. Task data is returned by the [tasks.task.get](../../tasks/tasks-task-get.md) method.

**User.** The `USER_ID` key indicates whose task list or whose personal planner is open. User data is returned by the [user.get](../../user/user-get.md) method.

**Workgroup and project.** The `GROUP_ID` key indicates which group the task list or the automation belongs to. Group data is returned by the [sonet_group.get](../../sonet-group/sonet-group-get.md) method.

**Call page.** The universal `URI` key contains the path of the Bitrix24 page the widget was opened from.

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| `placement.bind` returns `Application context required` | Register the placement on behalf of an application. A placement cannot be bound with a webhook ||
|| The widget is registered but does not appear in the interface | Complete the [application installation](../../../settings/app-installation/installation-finish.md) and reload the page ||
|| The widget does not appear in the task card | Check the `groupId` connection parameter: if it is filled in, the widget is displayed only in tasks of the listed projects ||
|| The handler does not find the task identifier | The key name depends on the placement: `ID` in the context menu of the list and `taskId` in the card ||
|| The item cannot be found above the task list | The menu is hidden under the button in the right part of the panel: it shows the *•••* icon or the name of the item opened last ||
|#

## Overview of Placements {#all-placements}

> Scope: [`placement, task`](../../scopes/permissions.md)

#|
|| **Placement** | **When to Use** ||
|| [TASK_LIST_CONTEXT_MENU](./list-context-menu.md) | Context menu item of a task in the list ||
|| [TASK_USER_LIST_TOOLBAR, TASK_GROUP_LIST_TOOLBAR](./list-toolbar.md) | Dropdown menu item above the task list of a user or a group ||
|| [TASK_ROBOT_DESIGNER_TOOLBAR](./robot-designer-toolbar.md) | Button in the task automation rules designer ||
|| [TASK_VIEW_TAB](./view-tab.md) | Widget in the task card, formerly a tab ||
|| [TASK_VIEW_SIDEBAR](./view-sidebar.md) | Widget in the task card, formerly the right panel ||
|| [TASK_VIEW_TOP_PANEL](./view-top-panel.md) | Widget in the task card, formerly a button in the top panel ||
|#

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../placement-list.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../../settings/interactivity/index.md)

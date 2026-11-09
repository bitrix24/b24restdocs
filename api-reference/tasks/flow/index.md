# Flows: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Flows automate the distribution and execution of tasks. An employee adds a task to the department's flow, and the flow assigns an assignee on its own. There is no need to look for someone to complete the task.

Flow efficiency is the share of tasks that are not overdue. It is calculated using the formula `(1 − <number of overdue tasks> / <total number of tasks in the flow>) * 100%`. A flow with no tasks has an efficiency of 100%. To improve it, change the task distribution type or the flow team.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Bitrix24 Flows: Getting Started](https://helpdesk.bitrix24.com/open/21415178/)

## Types of Distribution

- **Manual distribution** `manually`. All tasks go to the flow moderator, who distributes them among employees.
- **Queue distribution** `queue`. The flow assigns tasks in turn to employees from the flow team — the list of responsible persons `responsibleList`. If the list includes a department, its employees receive the tasks. The flow skips an employee who is absent from work, for example on vacation or sick leave.
- **Self-assignment** `himself`. A task is assigned to the creator, and each team member receives a notification. The member who clicks the "Accept task" button becomes the assignee. The task status changes to "In progress".

If, with queue distribution or self-assignment, no one from the team is at work, the flow switches to manual distribution. The flow administrator becomes the moderator and receives a notification.

## Relationships with Other Objects

**Group.** A flow is linked to a group, whose identifier is passed in the `groupId` parameter. If no group is specified when creating a flow, a new one is created. The group identifier is returned by the [get list of groups](../../sonet-group/socialnetwork-api-workgroup-list.md) method or the [create group](../../sonet-group/sonet-group-create.md) method.

**User.** A flow has an administrator, whose identifier is passed in the `ownerId` parameter. If no administrator is specified when creating a flow, the flow creator becomes the administrator. The user identifier is returned by the [user.get](../../user/user-get.md) method.

**Task Template.** Flow tasks can be created from a template — the template identifier is passed in the `templateId` parameter.

{% note tip "User Documentation" %}

- [Create a workgroup or project](https://helpdesk.bitrix24.com/open/22796428/)
- [Tasks in Bitrix24](https://helpdesk.bitrix24.com/open/18034564/)
- [Task templates](https://helpdesk.bitrix24.com/open/20883624/)

{% endnote %}

## How to Add a Task to the Flow

To add a task to a flow, pass the flow identifier in the `FLOW_ID` parameter of the [create new task](../tasks-task-add.md) or [update existing task](../tasks-task-update.md) method. Bitrix24 fills in the other fields the flow needs on its own, according to the flow settings.

Without a flow, a new task requires a title and an assignee. For a task in an enabled flow, a title and `FLOW_ID` are enough: the flow assigns the assignee.

{% note tip "User Documentation" %}

- [Create and find tasks in Flows](https://helpdesk.bitrix24.com/open/21438552/)

{% endnote %}

## How to Enable or Disable a Flow

You can temporarily disable a flow and enable it again later with the [tasks.flow.Flow.activate](./tasks-flow-flow-activate.md) method. While a flow is disabled, it does not assign assignees to new tasks. Tasks already in the flow remain in it.

The method accepts the flow identifier `flowId`. It is returned by the [create new flow](./tasks-flow-flow-create.md) method, and for a task from a flow — by the [get task information](../tasks-task-get.md) method.

## How to Check Flow Name Uniqueness

The flow name must be unique. The [tasks.flow.Flow.isExists](./tasks-flow-flow-is-exists.md) method checks whether a name is already taken.

## Limitations

- A flow must have at least one responsible person: when creating a flow, the `responsibleList` parameter is required for any distribution type
- The time allotted to complete a task, `plannedCompletionTime`, is at least one hour
- The [tasks.flow.Flow.update](./tasks-flow-flow-update.md) method resets the description, flags, and notifications missing from the request to their default values: the description is cleared and notifications are turned off. Before updating, retrieve the current values with the [tasks.flow.Flow.get](./tasks-flow-flow-get.md) method

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - create a flow — a Bitrix24 administrator or a user with permission to create flows, except extranet users
> - update, delete, enable, or disable a flow — a Bitrix24 administrator, the flow creator, or the flow administrator
> - retrieve and pin a flow — a user who can see the flow: a Bitrix24 administrator, a flow member, or an employee of a department specified in the flow. If all users can add tasks to the flow, all employees can see it
> - check a name — any user

#| 
|| **Method** | **Description** ||
|| [tasks.flow.Flow.create](./tasks-flow-flow-create.md) | Creates a flow ||
|| [tasks.flow.Flow.update](./tasks-flow-flow-update.md) | Updates a flow ||
|| [tasks.flow.Flow.get](./tasks-flow-flow-get.md) | Retrieves a flow ||
|| [tasks.flow.Flow.delete](./tasks-flow-flow-delete.md) | Deletes a flow ||
|| [tasks.flow.Flow.isExists](./tasks-flow-flow-is-exists.md) | Checks whether a flow with this name exists ||
|| [tasks.flow.Flow.activate](./tasks-flow-flow-activate.md) | Enables or disables a flow ||
|| [tasks.flow.Flow.pin](./tasks-flow-flow-pin.md) | Pins or unpins a flow in the list ||
|#

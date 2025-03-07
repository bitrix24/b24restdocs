# Flows: Overview of Methods

Flows are a tool that automates the distribution and execution of tasks. Employees do not need to search for who will complete a task. They assign tasks to the department's flow, and it automatically assigns an assignee.

In the flow, you can track the efficiency of task completion. Efficiency is calculated using the formula: `<number of overdue tasks> / <total number of tasks in the flow> * 100%`. To improve efficiency, change the type of task distribution or the flow team.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Bitrix24 Flows: Getting Started](https://helpdesk.bitrix24.com/open/21415178/) 

## Types of Distribution

- **Manual distribution** `manually`. All tasks are assigned to the flow moderator. The moderator distributes tasks among employees.
- **Queue distribution** `queue`. Tasks are assigned in turn to each team member. A member will be skipped if they are absent from work, for example, on vacation or sick leave.
- **Self-assignment** `himself`. Tasks are assigned to the creator. Each team member in the flow will receive a notification. A flow member will become the assignee if they click the Take to Work button. If a task is taken to work, the system will change the task status to In Progress.

If there are no employees left in the flow team with queue distribution or self-assignment, the flow will automatically switch to manual distribution mode. The flow administrator will become the moderator and receive a notification.

## Flow Connection with Other Objects

**Group.** The flow is linked to a group by the identifier `groupId`. The identifier can be obtained using the method [creating a new group](../../sonet-group/sonet-group-create.md) or the method [getting a list of groups](../../sonet-group/socialnetwork-api-workgroup-list.md). If the group identifier is not specified when creating the flow, a new group will be created.

**User.** The flow is linked to a user by the flow administrator's identifier `ownerId`. The user identifier can be obtained using the method [user.get](../../user/user-get.md). If the identifier is not specified when creating the flow, the creator will be the flow administrator.

**Task Template.** Flow tasks are created based on a template. The template identifier is specified in `templateId`.

{% note tip "User Documentation" %}

- [How to Create a Group and Project](https://helpdesk.bitrix24.com/open/22699004/)
- [Bitrix24 Tasks](https://helpdesk.bitrix24.com/open/17962166/)
- [Task Templates](https://helpdesk.bitrix24.com/open/17869536/)

{% endnote %}

## How to Add a Task to the Flow

You can add a task to the flow using the method [creating a new task](../tasks-task-add.md) or the method [updating an existing task](../tasks-task-update.md). In the `FLOW_ID` parameter, specify the flow identifier.

Bitrix24 will automatically fill in all the necessary fields for the flow according to the flow settings.

The title and assignee are required fields when creating a task outside the flow. In the case of tasks in the flow, it is sufficient to fill in `FLOW_ID` and the title. The flow will assign the assignee.

{% note tip "User Documentation" %}

- [How to Create and Find a Task in the Flow](https://helpdesk.bitrix24.com/open/21307012/)

{% endnote %}

## Enable or Disable the Flow

If you need to temporarily stop working with the flow, you can disable it. The flow can be enabled or disabled using the method [tasks.flow.Flow.activate](./tasks-flow-flow-activate.md) by the identifier `flowId`. 

You can obtain the flow identifier using the method [creating a new flow](./tasks-flow-flow-create.md) or the method [getting task information](../tasks-task-get.md) for a task from the flow.

## Check Flow Name Uniqueness

The flow name must be unique. You can check the uniqueness of the name using the method [tasks.flow.Flow.isExists](./tasks-flow-flow-is-exists.md).

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [tasks.flow.Flow.create](./tasks-flow-flow-create.md) | Create a flow ||
|| [tasks.flow.Flow.update](./tasks-flow-flow-update.md) | Modify a flow ||
|| [tasks.flow.Flow.get](./tasks-flow-flow-get.md) | Get a flow ||
|| [tasks.flow.Flow.delete](./tasks-flow-flow-delete.md) | Delete a flow ||
|| [tasks.flow.Flow.isExists](./tasks-flow-flow-is-exists.md) | Check if a flow with that name exists ||
|| [tasks.flow.Flow.activate](./tasks-flow-flow-activate.md) | Enable or disable a flow ||
|| [tasks.flow.Flow.pin](./tasks-flow-flow-pin.md) | Pin or unpin a flow in the list ||
|#
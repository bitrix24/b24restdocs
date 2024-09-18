# Flows

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

Flows are a tool for automating the distribution and execution of tasks. They allow for automatic assignment of participants, simplifying process management. Users place tasks into the flow, and they do not need to find out who and how will perform the task — they will receive the result.

The tool allows for timely adjustments to processes to achieve maximum flow efficiency — for example, changing the type of distribution or modifying the flow team. Efficiency is calculated using the formula: `<number of overdue tasks> / <total number of tasks in the flow> * 100%`

## Types of Distribution

- Manual Distribution (`manually`)
All tasks will be assigned to the flow moderator. They will distribute tasks among employees.

- Queue Distribution (`queue`)
Tasks will be assigned in turn to each team participant. If a participant is unavailable (on vacation, sick leave, etc.), they will be skipped.

If there are no employees left in the team for a flow with queue distribution (all on vacation, terminated, or removed), the flow automatically switches to manual distribution mode, with the flow administrator being assigned as the moderator, who receives the corresponding notification.

### Adding a Task to the Flow

To add an existing task to the flow, use the method [tasks.task.update](../tasks-task-update.md) and pass the `FLOW_ID` parameter with the identifier of the desired flow.

To create a new task in the flow, use the method [tasks.task.add](../tasks-task-add.md) and pass the `FLOW_ID` parameter with the flow identifier.

In both cases, the necessary fields for the flow (group, assignee, deadline, etc.) will be automatically filled in according to the flow.

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [tasks.flow.flow.create](./tasks-flow-flow-create.md) | Create a flow ||
|| [tasks.flow.flow.get](./tasks-flow-flow-get.md) | Get a flow ||
|| [tasks.flow.flow.update](./tasks-flow-flow-update.md) | Update a flow ||
|| [tasks.flow.flow.delete](./tasks-flow-flow-delete.md) | Delete a flow ||
|| [tasks.flow.flow.isExists](./tasks-flow-flow-is-exists.md) | Check if a flow with that name exists ||
|| [tasks.flow.flow.activate](./tasks-flow-flow-activate.md) | Enable/disable a flow ||
|#
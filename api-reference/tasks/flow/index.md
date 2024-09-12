# Flows

{% if build == 'dev' %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

Flows are a tool for automating the distribution and execution of tasks. It allows for automatic assignment of participants, simplifying process management. Users place tasks into the flow, and they do not need to find out who and how will perform the task - they will receive the result.
The tool allows for timely adjustments to processes to achieve maximum flow efficiency, such as changing the distribution type or modifying the flow team. Efficiency is calculated using the formula: `<number of overdue tasks> / <total number of tasks in the flow> * 100%`

## Distribution Types

- Manual distribution (`manually`)
All tasks will be assigned to the flow moderator. They will distribute tasks among employees.
- Queue distribution (`queue`)
Tasks will be assigned in turn to each team participant. If a participant is absent (on vacation, sick leave, etc.), they will be skipped.

Note:
If there are no employees left in the team for a flow with queue distribution (all on vacation, terminated, or removed), the flow will automatically switch to manual distribution mode, with the flow administrator assigned as the moderator, who will receive the corresponding notification.

### Adding a Task to the Flow
To add an existing task to the flow, you can use the method [tasks.task.update](../tasks-task-update.md), passing the FLOW_ID parameter as the identifier of the desired flow.
To create a new task in the flow, you can use the method [tasks.task.add](../tasks-task-add.md), passing the FLOW_ID parameter as the flow identifier.
In both cases, the necessary fields for the flow (group, assignee, deadline, etc.) will be automatically filled in according to the flow.

## Methods

#|
|| **Method** | **Description** ||
|| [tasks.flow.flow.get](./tasks-flow-flow-get.md) | Retrieve the flow. ||
|| [tasks.flow.flow.create](./tasks-flow-flow-create.md) | Create a flow. ||
|| [tasks.flow.flow.update](./tasks-flow-flow-update.md) | Modify the flow. ||
|| [tasks.flow.flow.delete](./tasks-flow-flow-delete.md) | Delete the flow. ||
|| [tasks.flow.flow.isExists](./tasks-flow-flow-is-exists.md) | Check if a flow with that name exists. ||
|| [tasks.flow.flow.activate](./tasks-flow-flow-activate.md) | Activate/deactivate the flow. ||
|#
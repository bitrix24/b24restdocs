# My Plan and Kanban Stages

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Write about what My Plan is, how it works, and provide a link to manage the stages of the working group
- Edits are needed to meet the writing standard

{% endnote %}

{% endif %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

My Plan is a tool for managing tasks and projects that allows users to organize and track their tasks in a Kanban board format. It displays tasks where the user is either a Participant or a Creator.

## Stages Table

#|
|| **Parameter** | **Description** ||
|| **ID** | Stage identifier ||
|| **TITLE** | Title ||
|| **SORT** | Sort index (read-only) ||
|| **COLOR** | Color in RGB format, example: FFAAEE. ||
|| **SYSTEM_TYPE** | System code, which can either be `null` or `NEW` - indicating that this stage is the default stage (read-only) ||
|| **ENTITY_TYPE** | Type of entity to which the stage belongs (`G` - group, `U` - user) (read-only, also specified when adding) ||
|| **ENTITY_ID** | ID of the entity to which the stage belongs (group or user) (read-only) ||
|#

## Task Retrieval

Task retrieval from stages is done using the method [task.task.get](../tasks-task-get.md). There are some specifics.

1. Retrieval from the Kanban stages of a group. In the filter array, pass the key `STAGE_ID` with the `ID` of the desired stage. However, if you are retrieving tasks from the default stage (which is first), you need to pass an array of values for `STAGE_ID` consisting of `0` and the `ID` of your stage (`[0, ID]`). Additionally, you need to filter this retrieval by group.
2. Retrieval from My Plan. In the filter array, pass the key `STAGES_ID` with the `ID` of the desired stage. There is no need to pass an array of values.

## Methods

#|
|| **Method** | **Description** ||
|| [task.stages.add](./task-stages-add.md) | This method adds Kanban / My Plan stages. ||
|| [task.stages.canmovetask](./task-stages-can-move-task.md) | This method determines whether the current user can move tasks in the specified entity. ||
|| [task.stages.delete](./task-stages-delete.md) | This method deletes Kanban / My Plan stages. ||
|| [task.stages.get](./task-stages-get.md) | This method retrieves Kanban / My Plan stages. ||
|| [task.stages.movetask](./task-stages-move-task.md) | This method moves tasks from one stage to another. ||
|| [task.stages.update](./task-stages-update.md) | This method updates Kanban / My Plan stages. ||
|#
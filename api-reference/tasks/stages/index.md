# My Plan and Kanban Stages

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

My Plan is a tool for managing tasks and projects that allows users to organize and track their tasks in a Kanban board format. It displays tasks where the user is either a Participant or a Creator.

## Methods

#|
|| **Method** | **Description** ||
|| [task.stages.add](./task-stages-add.md) | This method adds stages to the Kanban / My Plan ||
|| [task.stages.update](./task-stages-update.md) | This method updates stages in the Kanban / My Plan ||
|| [task.stages.get](./task-stages-get.md) | This method retrieves stages from the Kanban / My Plan ||
|| [task.stages.canmovetask](./task-stages-can-move-task.md) | This method determines if the current user can move tasks in the specified entity ||
|| [task.stages.movetask](./task-stages-move-task.md) | This method moves tasks from one stage to another ||
|| [task.stages.delete](./task-stages-delete.md) | This method deletes stages from the Kanban / My Plan ||
|#

## Task Retrieval

Tasks can be retrieved from stages using the [task.task.get](../tasks-task-get.md) method. There are some specifics:

1. Retrieval from Kanban group stages.
   In the filter array, pass the key `STAGE_ID` with the `ID` of the desired stage. However, if you are retrieving tasks from the default stage (which is the first one), you need to pass an array of values for `STAGE_ID` consisting of `0` and the `ID` of your stage (`[0, ID]`). Additionally, you need to filter this retrieval by group.
2. Retrieval from My Plan.
   In the filter array, pass the key `STAGES_ID` with the `ID` of the desired stage. There is no need to pass an array of values.
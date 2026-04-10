# Methods for Working with Tasks (item)

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods `task.item.*` has been halted. 
Please refer to the section [Current Task Methods (`tasks.task.*`)](../../index.md).

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Allowed Fields

#|
|| **Name** | **Description** | **Read** | **Write** | **Sort** | **Filter** ||
|| TITLE | Task name | + | + | + | + ||
|| DESCRIPTION | Task description | + | + | | ||
|| DEADLINE | Deadline | + | + | + | ||
|| START_DATE_PLAN | Planned start date | + | + | + | + ||
|| END_DATE_PLAN | Planned end date | + | + | | + ||
|| PRIORITY | Priority | + | + | + | + ||
|| ACCOMPLICES | Participants (user identifiers) | + | + | | ||
|| ACCOMPLICE | Participants (field used for filtering) | | | | + ||
|| AUDITORS | Observers (user identifiers) | + | + | | ||
|| AUDITOR | Observers (field used for filtering) | | | | + ||
|| TAGS | Tags (when added — simply an array of tags in text form). `CTasks::GetList()` does not return tag fields. `CTaskItem::getInstance()->getTags()` returns an array of tag names | + | + | | ||
|| TAG | Tags (field used for filtering) | | | | + ||
|| ALLOW_CHANGE_DEADLINE | Flag "Allow the executor (responsible) to change the deadline" | + | + | + | ||
|| TASK_CONTROL | Flag "Accept work after task completion" | + | + | | ||
|| PARENT_ID | Identifier of the parent task | + | + | | + ||
|| DEPENDS_ON | Identifier of the previous task | + | + | | + ||
|| GROUP_ID | Identifier of the working group | + | + | + | + ||
|| RESPONSIBLE_ID | Identifier of the executor | + | + | + | + ||
|| TIME_ESTIMATE | Planned labor costs | + | + | + | + ||
|| ID | Task identifier. Unique within the database | + | | + | + ||
|| CREATED_BY | Identifier of the Creator | + | + | + | + ||
|| DESCRIPTION_IN_BBCODE | Is the task description stored in BB codes | + | | | ||
|| DECLINE_REASON | Reason for task rejection | + | + | | ||
|| STATUS | Task status | + | + | + | + ||
|| RESPONSIBLE_NAME | Executor's first name | + | | | + ||
|| RESPONSIBLE_LAST_NAME | Executor's last name | + | | | ||
|| RESPONSIBLE_SECOND_NAME | Executor's middle name | + | | | ||
|| DATE_START | Date when the task execution started | + | | + | + ||
|| DURATION_FACT | Time spent on the task in minutes | + | | | ||
|| DURATION_PLAN | Planned duration in hours or days | + | + | | ||
|| DURATION_TYPE | Unit type for planned duration: days, hours, or minutes | + | + | | ||
|| CREATED_BY_NAME | Creator's first name | + | | | ||
|| CREATED_BY_LAST_NAME | Creator's last name | + | | | ||
|| CREATED_BY_SECOND_NAME | Creator's middle name | + | | | ||
|| CREATED_DATE | Task creation date | + | + | + | + ||
|| CHANGED_BY | User who last modified the task (user identifier) | + | + | | + ||
|| CHANGED_DATE | Date of the last task modification | + | + | + | + ||
|| STATUS_CHANGED_BY | User who changed the task status (user identifier) | + | + | | + ||
|| STATUS_CHANGED_DATE | Date of status change | + | + | | ||
|| CLOSED_BY | Who completed the task | + | | | ||
|| CLOSED_DATE | Date of task completion | + | | + | + ||
|| GUID | Globally unique identifier | + | | | + ||
|| MARK | Task rating. Possible values: `P` (positive) and `N` (negative) | + | + | + | + ||
|| VIEWED_DATE | Date of the last view of the task in the public interface by the current user | + | | | ||
|| TIME_SPENT_IN_LOGS | Time spent on the task in seconds | + | | | ||
|| FAVORITE | Presence in Favorites for the current user | + | | + | + ||
|| ALLOW_TIME_TRACKING | Is time tracking enabled for the task | + | + | + | + ||
|| ADD_IN_REPORT | Is the task included in the performance report | + | + | | + ||
|| FORUM_ID | Identifier of the forum where comments to the task are stored | + | | | ||
|| FORUM_TOPIC_ID | Identifier of the forum topic where comments to the task are stored | + | | | + ||
|| COMMENTS_COUNT | Number of comments on the task | + | | | ||
|| SITE_ID | Identifier of the site. By default, this field records the identifier of the site where the task is created | + | + | | + ||
|| SUBORDINATE | Is any of the task participants a subordinate of the current user | + | | | ||
|| FORKED_BY_TEMPLATE_ID | Identifier of the template based on which the task was automatically created. For some old tasks, this may not be set | + | | | ||
|| MULTITASK | Was the task created for multiple executors | + | | | ||
|| ONLY_ROOT_TASKS | Field that allows selecting only those tasks that either have no parent task or have one, but we do not have access to that parent task | | | | + ||
|| MATCH_WORK_TIME | Should the execution dates and deadline always be set during working hours | + | + | + | + ||
|#

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [task.item.add](./task-item-add.md) | Creates a new task ||
|| [task.item.delete](./task-item-delete.md) | Deletes a task ||
|| [task.item.getdata](./task-item-get-data.md) | Returns an array of task data ||
|| [task.item.getmanifest](./task-item-get-manifest.md) | Returns a list of `task.item.*` methods with their descriptions ||
|| [task.item.list](./task-item-list.md) | Returns a list of tasks ||
|| [task.item.update](./task-item-update.md) | Updates task data ||
|| [task.item.getdescription](./task-item-get-description.md) | Returns the task description ||
|| [task.item.getfiles](./task-item-get-files.md) | Returns an array with links to files attached to the task ||
|| [task.item.getdependson](./task-item-get-dependson.md) | Returns an array with identifiers of tasks that the task depends on ||
|| [task.item.getallowedactions](./task-item-get-allowed-actions.md) | Returns an array of identifiers of allowed actions on the task ||
|| [task.item.getallowedtaskactionsasstrings](./task-item-get-allowed-task-actions-as-strings.md) | Returns a list of allowed actions on the task ||
|| [task.item.isactionallowed](./task-item-is-action-allowed.md) | Checks if the action is allowed ||
|| [task.item.delegate](./task-item-delegate.md) | Delegates the task to a new user ||
|| [task.item.startexecution](./task-item-start-execution.md) | Changes the task status to "in progress" ||
|| [task.item.defer](./task-item-defer.md) | Changes the task status to "deferred" ||
|| [task.item.complete](./task-item-complete.md) | Changes the task status to "completed" or "conditionally completed (awaiting executor control)" ||
|| [task.item.renew](./task-item-renew.md) | Changes the task status to "not executed" ||
|| [task.item.approve](./task-item-approve.md) | Changes a task awaiting control to "completed" ||
|| [task.item.disapprove](./task-item-disapprove.md) | Changes a task awaiting control to "not executed" ||
|| [task.item.addtofavourite](./task-item-add-to-favourite.md) | Adds the task to Favorites ||
|| [task.item.deletefromfavorite](./task-item-delete-from-favorite.md) | Removes the task from Favorites ||
|| [task.item.addfile](./task-item-add-file.md) | Uploads a file to the task ||
|| [task.item.deletefile](./task-item-delete-file.md) | Removes the file attachment from the task ||
|| [task.logitem.list](./task-log-item-list.md) | Returns the history of task changes ||
|#

{% note info %}

To obtain the tags of a specific task, you need to pass the parameter `/rest/task.item.gettags.xml?TASK_ID=3&auth=18tci5kga6v12g8okzm5r26sv0n9is84`. The request can be either `ID` or `TASK_ID`. It is essential that this parameter is first. The response will return `{"result":["TAG1","TAG2","ETC..."]}`.

{% endnote %}
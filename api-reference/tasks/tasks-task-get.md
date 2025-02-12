# Get Task by ID tasks.task.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - **curl, js, php)
- no error response
- no success response

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.get` returns information about a specific task.

{% note warning %}

You must specify fields in `select`, as default fields may change in the future.

{% endnote %}

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|| **select**
[`unknown`](../data-types.md) | Array of record fields that will be returned by the method. You can specify only the fields that are necessary.

The field can take the following values: 
- **ID** — task identifier; 
- **PARENT_ID** — parent task identifier; 
- **TITLE** — task title; 
- **DESCRIPTION** — description; 
- **MARK** — rating; 
- **PRIORITY** — priority:
    - **0** — low;
    - **1** — medium;
    - **2** — high;
- **STATUS** — status; 
- **MULTITASK** — multiple task; 
- **NOT_VIEWED** — unviewed task; 
- **REPLICATE** — recurring task; 
- **GROUP_ID** — working group; 
- **STAGE_ID** — stage; 
- **CREATED_BY** — creator; 
- **CREATED_DATE** — creation date; 
- **RESPONSIBLE_ID** — executor; 
- **ACCOMPLICE** — participant identifier; 
- **AUDITOR** — auditor identifier; 
- **CHANGED_BY** — who changed the task; 
- **CHANGED_DATE** — date of change; 
- **STATUS_CHANGED_DATE** — who changed the status; 
- **CLOSED_BY** — who closed the task; 
- **CLOSED_DATE** — task closure date; 
- **DATE_START** — start date; 
- **DEADLINE** — deadline; 
- **START_DATE_PLAN** — planned start; 
- **END_DATE_PLAN** — planned completion; 
- **GUID** — GUID (statistically unique 128-bit identifier); 
- **XML_ID** — external code; 
- **COMMENTS_COUNT** — number of comments; 
- **NEW_COMMENTS_COUNT** — number of new comments; 
- **TASK_CONTROL** — take into work; 
- **ADD_IN_REPORT** — add to report; 
- **FORKED_BY_TEMPLATE_ID** — created automatically from a template; 
- **TIME_ESTIMATE** — time allocated for the task; 
- **TIME_SPENT_IN_LOGS** — time spent from change history; 
- **MATCH_WORK_TIME** — skip weekends; 
- **FORUM_TOPIC_ID** — forum topic identifier; 
- **FORUM_ID** — forum identifier; 
- **SITE_ID** — site identifier; 
- **SUBORDINATE** — subordinate task; 
- **FAVORITE** — Favorites; 
- **VIEWED_DATE** — date of last view; 
- **SORTING** — sorting index; 
- **DURATION_PLAN** — spent (planned); 
- **DURATION_FACT** — spent (actual); 
- **DURATION_TYPE** — duration type:
    - **0** — seconds
    - **1** — minutes
    - **2** — hours
    - **3** — days
    - **4** — weeks
    - **5** — months
    - **6** — years
- **UF_CRM_TASK** — binding to CRM entities.

By default, all non-computed fields of the main query table will be returned.

To obtain custom fields and the binding field to CRM entities (`UF_CRM_TASK`), they must be specified directly in `SELECT`. The list of fields can be clarified by sending a request [tasks.task.getFields](./tasks-task-get-fields.md). ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'tasks.task.get',
        {taskId:1, select:['ID','TITLE']},
        function(res){console.log(res.answer.result);}
    );
    ```

{% endlist %}

To get the tags of a specific task:

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'tasks.task.get',
        {taskId:1367, select:['TAGS']},
        function(res){console.log(res.answer.result);}
    );
    ```

{% endlist %}

Syntax for selecting all fields:

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'tasks.task.get',
        {taskId:1367, select:['*']},
        function(res){console.log(res.answer.result);}
    )
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../tutorials/tasks/how-to-connect-task-to-spa.md)

{% include [Footnote on examples](../../_includes/examples.md) %}
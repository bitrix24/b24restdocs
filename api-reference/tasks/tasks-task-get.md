# Get Task by ID tasks.task.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (should include three examples - **curl, js, php**)
- no error response provided
- no success response provided

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

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
- **GROUP_ID** — workgroup; 
- **STAGE_ID** — stage; 
- **CREATED_BY** — creator; 
- **CREATED_DATE** — creation date; 
- **RESPONSIBLE_ID** — assignee; 
- **ACCOMPLICE** — co-executor identifier; 
- **AUDITOR** — observer identifier; 
- **CHANGED_BY** — who modified the task; 
- **CHANGED_DATE** — modification date; 
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
- **TIME_ESTIMATE** — estimated time; 
- **TIME_SPENT_IN_LOGS** — time spent from change history; 
- **MATCH_WORK_TIME** — skip weekends; 
- **FORUM_TOPIC_ID** — forum topic identifier; 
- **FORUM_ID** — forum identifier; 
- **SITE_ID** — site identifier; 
- **SUBORDINATE** — subordinate task; 
- **FAVORITE** — Favorites; 
- **VIEWED_DATE** — last viewed date; 
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

By default, all non-computed fields from the main query table will be returned.

To retrieve custom fields and the binding field to CRM entities (`UF_CRM_TASK`), they must be specified directly in `SELECT`. The list of fields can be clarified by sending a request to [tasks.task.getFields](./tasks-task-get-fields.md). ||
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

To retrieve tags for a specific task:

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

Syntax for retrieving all fields:

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

{% include [Footnote on examples](../../_includes/examples.md) %}
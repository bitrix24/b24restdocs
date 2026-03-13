# Get Task Template by ID tasks.template.get

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: user with permission to view the template

The method `tasks.template.get` returns the task template data by its ID.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../data-types.md) | The ID of the task template.

The task template ID can be obtained when [creating a new template](./tasks-template-add.md) ||
|| **params**
[`object`](../../data-types.md) | Additional selection parameters [(detailed description)](#params) ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to return. 

By default, `['*', 'UF_*']` is used — select all fields and custom fields ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.get',
            {
                templateId: 139
            }
        );

        const result = response.getData().result;
        console.log(result);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.template.get',
                [
                    'templateId' => 139
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);

    } catch (Throwable $e) {
        echo 'Error retrieving template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.template.get',
        {
            templateId: 139
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.template.get',
        [
            'templateId' => 139
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": "139",
        "TITLE": "Preparation of Weekly Project Status",
        "DESCRIPTION": "Task template for preparing and agreeing on the weekly project status with the team and manager",
        "DESCRIPTION_IN_BBCODE": "Y",
        "PRIORITY": "2",
        "STATUS": "1",
        "STAGE_ID": "0",
        "RESPONSIBLE_ID": "102",
        "DEADLINE_AFTER": null,
        "START_DATE_PLAN_AFTER": "32400",
        "END_DATE_PLAN_AFTER": "97200",
        "REPLICATE": "Y",
        "CREATED_BY": "101",
        "XML_ID": null,
        "ALLOW_CHANGE_DEADLINE": "Y",
        "ALLOW_TIME_TRACKING": "Y",
        "TASK_CONTROL": "N",
        "ADD_IN_REPORT": "N",
        "GROUP_ID": "0",
        "PARENT_ID": "8131",
        "MULTITASK": "N",
        "SITE_ID": "s1",
        "ACCOMPLICES": "a:0:{}",
        "AUDITORS": "a:0:{}",
        "RESPONSIBLES": "a:1:{i:0;s:3:\"102\";}",
        "FILES": null,
        "TAGS": null,
        "DEPENDS_ON": null,
        "MATCH_WORK_TIME": "Y",
        "TASK_ID": null,
        "TPARAM_TYPE": "0",
        "TPARAM_REPLICATION_COUNT": "0",
        "REPLICATE_PARAMS": "a:19:{s:6:\"PERIOD\";s:6:\"weekly\";s:12:\"WORKDAY_ONLY\";s:1:\"N\";s:9:\"WEEK_DAYS\";a:1:{i:0;i:2;}s:4:\"TIME\";s:5:\"11:00\";s:15:\"TIMEZONE_OFFSET\";i:0;s:11:\"REPEAT_TILL\";s:7:\"endless\";s:10:\"START_DATE\";s:19:\"16.03.2026 00:00:00\";s:9:\"EVERY_DAY\";i:1;s:10:\"EVERY_WEEK\";i:1;s:15:\"MONTHLY_DAY_NUM\";i:1;s:19:\"MONTHLY_MONTH_NUM_1\";i:1;s:19:\"MONTHLY_MONTH_NUM_2\";i:1;s:14:\"YEARLY_DAY_NUM\";i:1;s:8:\"END_DATE\";N;s:12:\"MONTHLY_TYPE\";i:1;s:11:\"YEARLY_TYPE\";i:1;s:14:\"DEADLINE_AFTER\";i:0;s:15:\"DEADLINE_OFFSET\";i:0;s:19:\"NEXT_EXECUTION_TIME\";s:19:\"17.03.2026 11:00:00\";}",
        "BASE_TEMPLATE_ID": "0",
        "TEMPLATE_CHILDREN_COUNT": "0",
        "CREATED_BY_NAME": "John",
        "CREATED_BY_LAST_NAME": "Doe",
        "CREATED_BY_SECOND_NAME": "Smith",
        "CREATED_BY_LOGIN": "john.doe@example.com",
        "CREATED_BY_WORK_POSITION": "Employee",
        "CREATED_BY_PHOTO": "10001",
        "RESPONSIBLE_NAME": "Peter",
        "RESPONSIBLE_LAST_NAME": "Peters",
        "RESPONSIBLE_SECOND_NAME": "Petrovich",
        "RESPONSIBLE_LOGIN": "peter.peters@example.com",
        "RESPONSIBLE_WORK_POSITION": "Employee",
        "RESPONSIBLE_PHOTO": "10002",
        "SCENARIO": "default",
        "UF_CRM_TASK": [
            "L_1179",
            "D_1833"
        ],
        "UF_TASK_WEBDAV_FILES": [
            1117
        ]
    },
    "time": {
        "start": 1773219184,
        "finish": 1773219184.579376,
        "duration": 0.5793759822845459,
        "processing": 0,
        "date_start": "2026-03-11T11:53:04+01:00",
        "date_finish": "2026-03-11T11:53:04+01:00",
        "operating_reset_at": 1773219784,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Task template data [(detailed description)](#result).

Returns `false`:
- if `templateId` is passed empty
- if a template with the specified `templateId` does not exist
- if the user does not have permission to view the template ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | The ID of the task template ||
|| **TITLE**
[`string`](../../data-types.md) | The title of the template ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | The description of the template ||
|| **DESCRIPTION_IN_BBCODE**
[`string`](../../data-types.md) | Indicator that the description is stored in BBCode format ||
|| **PRIORITY**
[`integer`](../../data-types.md) | The priority of the task ||
|| **STATUS**
[`integer`](../../data-types.md) | The status of the template ||
|| **STAGE_ID**
[`integer`](../../data-types.md) | The ID of the stage ||
|| **RESPONSIBLE_ID**
[`integer`](../../data-types.md) | The ID of the responsible person ||
|| **DEADLINE_AFTER**
[`datetime`](../../data-types.md) | Deadline offset relative to the creation date of the task template ||
|| **START_DATE_PLAN_AFTER**
[`string`](../../data-types.md) | Planned start date offset in seconds ||
|| **END_DATE_PLAN_AFTER**
[`string`](../../data-types.md) | Planned end date offset in seconds ||
|| **REPLICATE**
[`string`](../../data-types.md) | Indicator of a recurring task ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | The ID of the Creator ||
|| **XML_ID**
[`string`](../../data-types.md) | External ID of the template ||
|| **ALLOW_CHANGE_DEADLINE**
[`string`](../../data-types.md) | Indicator that the responsible person can change the deadline ||
|| **ALLOW_TIME_TRACKING**
[`string`](../../data-types.md) | Indicator of time tracking for tasks based on the template ||
|| **TASK_CONTROL**
[`string`](../../data-types.md) | Indicator of the need to accept the work ||
|| **ADD_IN_REPORT**
[`string`](../../data-types.md) | Indicator of including the task in the performance report ||
|| **GROUP_ID**
[`integer`](../../data-types.md) | The ID of the project ||
|| **PARENT_ID**
[`integer`](../../data-types.md) | The ID of the parent template ||
|| **MULTITASK**
[`string`](../../data-types.md) | Indicator of a multi-task ||
|| **SITE_ID**
[`string`](../../data-types.md) | The ID of the site ||
|| **ACCOMPLICES**
[`string`](../../data-types.md) | Serialized list of Participants ||
|| **AUDITORS**
[`string`](../../data-types.md) | Serialized list of auditors ||
|| **RESPONSIBLES**
[`string`](../../data-types.md) | Serialized list of responsibles ||
|| **FILES**
[`array`](../../data-types.md) | List of template files ||
|| **TAGS**
[`array`](../../data-types.md) | List of template tags ||
|| **DEPENDS_ON**
[`array`](../../data-types.md) | List of template dependencies ||
|| **MATCH_WORK_TIME**
[`string`](../../data-types.md) | Indicator of considering working time when calculating deadlines ||
|| **TASK_ID**
[`integer`](../../data-types.md) | The ID of the related task ||
|| **TPARAM_TYPE**
[`integer`](../../data-types.md) | Type of additional parameters for the template ||
|| **TPARAM_REPLICATION_COUNT**
[`integer`](../../data-types.md) | Number of tasks already created based on the replication rule ||
|| **REPLICATE_PARAMS**
[`string`](../../data-types.md) | Serialized replication parameters ||
|| **BASE_TEMPLATE_ID**
[`integer`](../../data-types.md) | The ID of the base template ||
|| **TEMPLATE_CHILDREN_COUNT**
[`integer`](../../data-types.md) | Number of child templates ||
|| **CREATED_BY_NAME**
[`string`](../../data-types.md) | Creator's first name ||
|| **CREATED_BY_LAST_NAME**
[`string`](../../data-types.md) | Creator's last name ||
|| **CREATED_BY_SECOND_NAME**
[`string`](../../data-types.md) | Creator's middle name ||
|| **CREATED_BY_LOGIN**
[`string`](../../data-types.md) | Creator's login ||
|| **CREATED_BY_WORK_POSITION**
[`string`](../../data-types.md) | Creator's position ||
|| **CREATED_BY_PHOTO**
[`integer`](../../data-types.md) | Creator's photo ID ||
|| **RESPONSIBLE_NAME**
[`string`](../../data-types.md) | Responsible person's first name ||
|| **RESPONSIBLE_LAST_NAME**
[`string`](../../data-types.md) | Responsible person's last name ||
|| **RESPONSIBLE_SECOND_NAME**
[`string`](../../data-types.md) | Responsible person's middle name ||
|| **RESPONSIBLE_LOGIN**
[`string`](../../data-types.md) | Responsible person's login ||
|| **RESPONSIBLE_WORK_POSITION**
[`string`](../../data-types.md) | Responsible person's position ||
|| **RESPONSIBLE_PHOTO**
[`integer`](../../data-types.md) | Responsible person's photo ID ||
|| **SCENARIO**
[`string`](../../data-types.md) | Template creation scenario ||
|| **UF_CRM_TASK**
[`array`](../../data-types.md) | Links to CRM objects ||
|| **UF_TASK_WEBDAV_FILES**
[`array`](../../data-types.md) | IDs of Drive files linked to the template ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {templateId}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-add.md)
- [{#T}](./tasks-template-update.md)
- [{#T}](./tasks-template-delete.md)
- [{#T}](./tasks-template-fields.md)
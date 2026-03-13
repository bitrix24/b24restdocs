# Add Task Template tasks.template.add

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: user with template creation permission

The method `tasks.template.add` creates a new task template.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Fields of the new task template [(detailed description)](./fields.md). 

Required fields for creating a template:
- `TITLE` — title of the task template
- `CREATED_BY` — ID of the Creator
- `RESPONSIBLE_ID` — ID of the Participant

||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "fields": {
        "PARENT_ID": 8131,
        "TITLE": "Preparation of weekly project status",
        "DESCRIPTION": "Task template for preparing and agreeing on the weekly project status with the team and manager",
        "PRIORITY": 2,
        "CREATED_BY": 101,
        "RESPONSIBLE_ID": 102,
        "REPLICATE": "Y",
        "START_DATE_PLAN_AFTER": "32400",
        "END_DATE_PLAN_AFTER": "97200",
        "REPLICATE_PARAMS": {
          "PERIOD": "weekly",
          "EVERY_WEEK": 1,
          "WEEK_DAYS": [2],
          "TIME": "11:00",
          "REPEAT_TILL": "endless",
          "START_DATE": "03/16/2026 00:00:00"
        },
        "UF_CRM_TASK": ["L_1179", "D_1833"]
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "fields": {
        "PARENT_ID": 8131,
        "TITLE": "Preparation of weekly project status",
        "DESCRIPTION": "Task template for preparing and agreeing on the weekly project status with the team and manager",
        "PRIORITY": 2,
        "CREATED_BY": 101,
        "RESPONSIBLE_ID": 102,
        "REPLICATE": "Y",
        "START_DATE_PLAN_AFTER": "32400",
        "END_DATE_PLAN_AFTER": "97200",
        "REPLICATE_PARAMS": {
          "PERIOD": "weekly",
          "EVERY_WEEK": 1,
          "WEEK_DAYS": [2],
          "TIME": "11:00",
          "REPEAT_TILL": "endless",
          "START_DATE": "03/16/2026 00:00:00"
        },
        "UF_CRM_TASK": ["L_1179", "D_1833"]
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.add',
            {
                fields: {
                    PARENT_ID: 8131,
                    TITLE: 'Preparation of weekly project status',
                    DESCRIPTION: 'Task template for preparing and agreeing on the weekly project status with the team and manager',
                    PRIORITY: 2,
                    CREATED_BY: 101,
                    RESPONSIBLE_ID: 102,
                    REPLICATE: 'Y',
                    START_DATE_PLAN_AFTER: '32400',
                    END_DATE_PLAN_AFTER: '97200',
                    REPLICATE_PARAMS: {
                        PERIOD: 'weekly',
                        EVERY_WEEK: 1,
                        WEEK_DAYS: [2],
                        TIME: '11:00',
                        REPEAT_TILL: 'endless',
                        START_DATE: '03/16/2026 00:00:00',
                    },
                    UF_CRM_TASK: ['L_1179', 'D_1833'],
                }
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
                'tasks.template.add',
                [
                    'fields' => [
                        'PARENT_ID' => 8131,
                        'TITLE' => 'Preparation of weekly project status',
                        'DESCRIPTION' => 'Task template for preparing and agreeing on the weekly project status with the team and manager',
                        'PRIORITY' => 2,
                        'CREATED_BY' => 101,
                        'RESPONSIBLE_ID' => 102,
                        'REPLICATE' => 'Y',
                        'START_DATE_PLAN_AFTER' => '32400',
                        'END_DATE_PLAN_AFTER' => '97200',
                        'REPLICATE_PARAMS' => [
                            'PERIOD' => 'weekly',
                            'EVERY_WEEK' => 1,
                            'WEEK_DAYS' => [2],
                            'TIME' => '11:00',
                            'REPEAT_TILL' => 'endless',
                            'START_DATE' => '03/16/2026 00:00:00',
                        ],
                        'UF_CRM_TASK' => ['L_1179', 'D_1833'],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Template ID: ' . $result;

    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.template.add',
        {
            fields: {
                PARENT_ID: 8131,
                TITLE: 'Preparation of weekly project status',
                DESCRIPTION: 'Task template for preparing and agreeing on the weekly project status with the team and manager',
                PRIORITY: 2,
                CREATED_BY: 101,
                RESPONSIBLE_ID: 102,
                REPLICATE: 'Y',
                START_DATE_PLAN_AFTER: '32400',
                END_DATE_PLAN_AFTER: '97200',
                REPLICATE_PARAMS: {
                    PERIOD: 'weekly',
                    EVERY_WEEK: 1,
                    WEEK_DAYS: [2], // Tuesday
                    TIME: '11:00',
                    REPEAT_TILL: 'endless',
                    START_DATE: '03/16/2026 00:00:00'
                },
                UF_CRM_TASK: ['L_1179', 'D_1833']
            },
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
        'tasks.template.add',
        [
            'fields' => [
                'PARENT_ID' => 8131,
                'TITLE' => 'Preparation of weekly project status',
                'DESCRIPTION' => 'Task template for preparing and agreeing on the weekly project status with the team and manager',
                'PRIORITY' => 2,
                'CREATED_BY' => 101,
                'RESPONSIBLE_ID' => 102,
                'REPLICATE' => 'Y',
                'START_DATE_PLAN_AFTER' => '32400',
                'END_DATE_PLAN_AFTER' => '97200',
                'REPLICATE_PARAMS' => [
                    'PERIOD' => 'weekly',
                    'EVERY_WEEK' => 1,
                    'WEEK_DAYS' => [2],
                    'TIME' => '11:00',
                    'REPEAT_TILL' => 'endless',
                    'START_DATE' => '03/16/2026 00:00:00',
                ],
                'UF_CRM_TASK' => ['L_1179', 'D_1833'],
            ],
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 109,
    "time": {
        "start": 1771937035,
        "finish": 1771937035.427759,
        "duration": 0.42775893211364746,
        "processing": 0,
        "date_start": "2026-02-24T15:43:55+01:00",
        "date_finish": "2026-02-24T15:43:55+01:00",
        "operating_reset_at": 1771937635,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created task template.

The method will not create a template and will return `"result": 0` if not all required fields are specified in `fields`: `TITLE`, `CREATED_BY`, `RESPONSIBLE_ID`  ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided or is empty ||
|| `400` | `0` | Access denied | Insufficient permissions to create the template ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-fields.md)
- [{#T}](./tasks-template-get.md)
- [{#T}](./tasks-template-update.md)
- [{#T}](./tasks-template-delete.md)
# Get the list of fields tasks.task.getFields

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.getFields` returns the description of standard and custom fields of a task.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.getFields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.getFields
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.getFields',
            {}
        );
        
        const result = response.getData().result;
        console.log('Task fields:', result);
        
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.getFields',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching task fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.getFields',
        {},
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.getFields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "fields": {
            "ID": {
                "title": "ID",
                "type": "integer",
                "primary": true
            },
            "PARENT_ID": {
                "title": "Base task ID",
                "type": "integer",
                "default": 0
            },
            "TITLE": {
                "title": "Title",
                "type": "string",
                "required": true
            },
            "DESCRIPTION": {
                "title": "Description",
                "type": "string"
            },
            "MARK": {
                "title": "Rating",
                "type": "enum",
                "values": {
                    "N": "Negative",
                    "P": "Positive"
                },
                "default": null
            },
            "PRIORITY": {
                "title": "Priority",
                "type": "enum",
                "values": {
                    "2": "High",
                    "1": "Medium",
                    "0": "Low"
                },
                "default": 1
            },
            "STATUS": {
                "title": "Status",
                "type": "enum",
                "values": {
                    "2": "Pending",
                    "3": "In Progress",
                    "4": "Awaiting Control",
                    "5": "Completed",
                    "6": "Deferred"
                },
                "default": 2
            },
            "MULTITASK": {
                "title": "Multitask",
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "NOT_VIEWED": {
                "title": null,
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "REPLICATE": {
                "title": "Recurring task",
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "GROUP_ID": {
                "title": "Project",
                "type": "integer",
                "default": 0
            },
            "STAGE_ID": {
                "title": "Stage",
                "type": "integer",
                "default": 0
            },
            "SPRINT_ID": {
                "title": "Sprint",
                "type": "integer",
                "default": 0
            },
            "BACKLOG_ID": {
                "title": "Backlog",
                "type": "integer",
                "default": 0
            },
            "CREATED_BY": {
                "title": "Creator",
                "type": "integer",
                "required": true
            },
            "CREATED_DATE": {
                "title": null,
                "type": "datetime"
            },
            "RESPONSIBLE_ID": {
                "title": "Assignee",
                "type": "integer",
                "required": true
            },
            "ACCOMPLICES": {
                "title": null,
                "type": "array"
            },
            "AUDITORS": {
                "title": null,
                "type": "array"
            },
            "CHANGED_BY": {
                "title": "Changed by",
                "type": "integer"
            },
            "CHANGED_DATE": {
                "title": "Change date",
                "type": "datetime"
            },
            "STATUS_CHANGED_BY": {
                "title": "Status changed by",
                "type": "integer"
            },
            "STATUS_CHANGED_DATE": {
                "title": "Status change date",
                "type": "datetime"
            },
            "CLOSED_BY": {
                "title": "Closed by",
                "type": "integer",
                "default": null
            },
            "CLOSED_DATE": {
                "title": "Closing date",
                "type": "datetime",
                "default": null
            },
            "ACTIVITY_DATE": {
                "title": null,
                "type": "datetime",
                "default": null
            },
            "DATE_START": {
                "title": "Start date",
                "type": "datetime",
                "default": null
            },
            "DEADLINE": {
                "title": "Deadline",
                "type": "datetime",
                "default": null
            },
            "START_DATE_PLAN": {
                "title": "Planned start",
                "type": "datetime",
                "default": null
            },
            "END_DATE_PLAN": {
                "title": "Planned completion",
                "type": "datetime",
                "default": null
            },
            "GUID": {
                "title": "GUID",
                "type": "string",
                "default": null
            },
            "XML_ID": {
                "title": "XML_ID",
                "type": "string",
                "default": null
            },
            "COMMENTS_COUNT": {
                "title": "Number of comments",
                "type": "integer",
                "default": 0
            },
            "SERVICE_COMMENTS_COUNT": {
                "title": null,
                "type": "integer",
                "default": 0
            },
            "NEW_COMMENTS_COUNT": {
                "title": null,
                "type": "integer",
                "default": 0
            },
            "ALLOW_CHANGE_DEADLINE": {
                "title": null,
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "ALLOW_TIME_TRACKING": {
                "title": null,
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "TASK_CONTROL": {
                "title": "Accept work",
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "ADD_IN_REPORT": {
                "title": "Add to report",
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "FORKED_BY_TEMPLATE_ID": {
                "title": "Created from template",
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "TIME_ESTIMATE": {
                "title": "Estimated time",
                "type": "integer"
            },
            "TIME_SPENT_IN_LOGS": {
                "title": "Time spent from change history",
                "type": "integer"
            },
            "MATCH_WORK_TIME": {
                "title": "Skip weekends",
                "type": "integer"
            },
            "FORUM_TOPIC_ID": {
                "title": "FORUM_TOPIC_ID",
                "type": "integer"
            },
            "FORUM_ID": {
                "title": "FORUM_ID",
                "type": "integer"
            },
            "SITE_ID": {
                "title": "SITE_ID",
                "type": "string"
            },
            "SUBORDINATE": {
                "title": "Subordinate task",
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": null
            },
            "FAVORITE": {
                "title": null,
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": null
            },
            "EXCHANGE_MODIFIED": {
                "title": "EXCHANGE_MODIFIED",
                "type": "datetime",
                "default": null
            },
            "EXCHANGE_ID": {
                "title": "EXCHANGE_ID",
                "type": "integer",
                "default": null
            },
            "OUTLOOK_VERSION": {
                "title": "OUTLOOK_VERSION",
                "type": "integer",
                "default": null
            },
            "VIEWED_DATE": {
                "title": "Last viewed date",
                "type": "datetime"
            },
            "SORTING": {
                "title": "Sorting index",
                "type": "double"
            },
            "DURATION_PLAN": {
                "title": "Spent (planned)",
                "type": "integer"
            },
            "DURATION_FACT": {
                "title": "Spent (actual)",
                "type": "integer"
            },
            "CHECKLIST": {
                "title": null,
                "type": "array"
            },
            "DURATION_TYPE": {
                "title": "DURATION_TYPE",
                "type": "enum",
                "values": [
                    "secs",
                    "mins",
                    "hours",
                    "days",
                    "weeks",
                    "monts",
                    "years"
                ],
                "default": "days"
            },
            "IS_MUTED": {
                "title": null,
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "IS_PINNED": {
                "title": null,
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "IS_PINNED_IN_GROUP": {
                "title": null,
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "FLOW_ID": {
                "title": "Flow",
                "type": "integer",
                "default": 0
            },
            "UF_CRM_TASK": {
                "title": "CRM entities",
                "type": "crm"
            },
            "UF_TASK_WEBDAV_FILES": {
                "title": "Upload files",
                "type": "disk_file"
            },
            "UF_MAIL_MESSAGE": {
                "title": null,
                "type": "mail_message"
            },
            "UF_NEW_TASKS_FIELD": {
                "title": "New task field",
                "type": "string"
            }
        }
    },
    "time": {
        "start": 1758790899,
        "finish": 1758790899.809331,
        "duration": 0.809330940246582,
        "processing": 0,
        "date_start": "2025-09-25T12:01:39+02:00",
        "date_finish": "2025-09-25T12:01:39+02:00",
        "operating_reset_at": 1758791499,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Title**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with [task field descriptions](./fields.md) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./tasks-task-get.md)
- [{#T}](./tasks-task-list.md)
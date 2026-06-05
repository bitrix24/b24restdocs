# Get the List of Fields tasks.task.getFields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.getFields` returns a description of standard and custom fields of a task.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // A single field descriptor and the map returned in result.fields
    type TaskFieldDescription = {
      title: string | null
      type: string
      required?: boolean
      primary?: boolean
      default?: unknown
      values?: Record<string, string> | string[]
    }
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskFieldsResult = {
      fields: Record<string, TaskFieldDescription>
    }

    try {
      // tasks.task.getFields takes no parameters
      const response = await $b24.actions.v2.call.make<TaskFieldsResult>({
        method: 'tasks.task.getFields',
        params: {},
        requestId: Text.getUuidRfc4122() // optional unique tracking id for this request
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const fields = response.getData()!.result.fields
        console.info(`Loaded ${Object.keys(fields).length} task fields`)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function loadTaskFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // tasks.task.getFields takes no parameters
          const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.getFields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122() // optional unique tracking id for this request
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const fields = response.getData().result.fields
          console.info(`Loaded ${Object.keys(fields).length} task fields`)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', loadTaskFields)
    </script>
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

HTTP Status: **200**

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
                "title": "Base Task ID",
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
                    "4": "Awaiting Review",
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
                "title": "Recurring Task",
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
                "title": "Participant",
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
                "title": "Modified By",
                "type": "integer"
            },
            "CHANGED_DATE": {
                "title": "Modification Date",
                "type": "datetime"
            },
            "STATUS_CHANGED_BY": {
                "title": "Status Changed By",
                "type": "integer"
            },
            "STATUS_CHANGED_DATE": {
                "title": "Status Change Date",
                "type": "datetime"
            },
            "CLOSED_BY": {
                "title": "Closed By",
                "type": "integer",
                "default": null
            },
            "CLOSED_DATE": {
                "title": "Closure Date",
                "type": "datetime",
                "default": null
            },
            "ACTIVITY_DATE": {
                "title": null,
                "type": "datetime",
                "default": null
            },
            "DATE_START": {
                "title": "Start Date",
                "type": "datetime",
                "default": null
            },
            "DEADLINE": {
                "title": "Deadline",
                "type": "datetime",
                "default": null
            },
            "START_DATE_PLAN": {
                "title": "Planned Start",
                "type": "datetime",
                "default": null
            },
            "END_DATE_PLAN": {
                "title": "Planned Completion",
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
                "title": "Number of Comments",
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
                "title": "Accept Work",
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "ADD_IN_REPORT": {
                "title": "Add to Report",
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "FORKED_BY_TEMPLATE_ID": {
                "title": "Created from Template",
                "type": "enum",
                "values": {
                    "Y": "Yes",
                    "N": "No"
                },
                "default": "N"
            },
            "TIME_ESTIMATE": {
                "title": "Estimated Time",
                "type": "integer"
            },
            "TIME_SPENT_IN_LOGS": {
                "title": "Time Spent from Change History",
                "type": "integer"
            },
            "MATCH_WORK_TIME": {
                "title": "Skip Weekends",
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
                "title": "Subordinate Task",
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
                "title": "Last Viewed Date",
                "type": "datetime"
            },
            "SORTING": {
                "title": "Sorting Index",
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
                    "months",
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
                "title": "CRM Entities",
                "type": "crm"
            },
            "UF_TASK_WEBDAV_FILES": {
                "title": "Upload Files",
                "type": "disk_file"
            },
            "UF_MAIL_MESSAGE": {
                "title": null,
                "type": "mail_message"
            },
            "UF_NEW_TASKS_FIELD": {
                "title": "New Task Field",
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
|| **Name**
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
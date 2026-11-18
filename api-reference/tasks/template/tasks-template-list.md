# Get Task Template List tasks.template.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.template.list` returns a list of task templates available to the user.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **params**
[`object`](../../data-types.md) | Selection parameters [(detailed description)](#params).

If `params` is not passed, the method returns all available templates with all fields ||
|| **start**
[`integer`](../../data-types.md) | Controls pagination.

The page size is always fixed at 50 records.

To get the second page of results, pass `50`. To get the third page, pass `100`, and so on.

The formula for calculating the `start` value:

`start = (N - 1) * 50`, where `N` is the number of the page you need ||
|#

### Parameter params {#params}

Only the following template fields can be used in `select`, `filter`, and `order`: `ID`, `TITLE`, `DESCRIPTION`, `DESCRIPTION_IN_BBCODE`, `PRIORITY`, `STATUS`, `RESPONSIBLE_ID`, `TIME_ESTIMATE`, `REPLICATE`, `CREATED_BY`, `XML_ID`, `ALLOW_CHANGE_DEADLINE`, `ALLOW_TIME_TRACKING`, `TASK_CONTROL`, `ADD_IN_REPORT`, `MATCH_WORK_TIME`, `GROUP_ID`, `PARENT_ID`, `MULTITASK`, `SITE_ID`, `REPLICATE_PARAMS`, `TAGS`, `ACCOMPLICES`, `AUDITORS`, `RESPONSIBLES`, `DEPENDS_ON`, `DEADLINE_AFTER`, `START_DATE_PLAN_AFTER`, `END_DATE_PLAN_AFTER`, `TASK_ID`, `STAGE_ID`, `TPARAM_TYPE`, `TPARAM_REPLICATION_COUNT`, `REPLICATE_TYPE`, `ZOMBIE`, `FILES`. The fields are described in [Task Template Fields](./fields.md).

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to return.

User fields `UF_*` and the fields `SE_CHECKLIST`, `SE_ACCESS`, `SE_TAG`, and `BASE_TEMPLATE_ID` cannot be specified in `select`. The method returns them only if `select` is not passed.

The value `*` is not supported: the method returns error `140`. To get all fields, do not pass `select` ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering templates in the format `{"field_1": "value", "field_2": "value"}`.

You can put an operator before the field name:
- `!` — not equal
- `>` — greater than
- `>=` — greater than or equal
- `<` — less than
- `<=` — less than or equal
- `%` — contains a substring

For example, `{">ID": 70, "REPLICATE": "N"}` selects non-recurring templates with an ID greater than 70 ||
|| **order**
[`object`](../../data-types.md) | An object for sorting in the format `{"field_1": "value", "field_2": "value"}`.

Possible values:
- `ASC` — ascending
- `DESC` — descending

If you pass any other value, the method returns an empty list ||
|#

Pass `select`, `filter`, and `order` only inside `params`. At the top level of the request, the method ignores them.

The method returns only the templates the user can view:
- templates the user created
- templates shared with the user in the template access settings
- templates of employees in the user's department, if the user has the "View own department's templates" permission
- templates of employees in other departments, if the user has the "View other departments' templates" permission

Bitrix24 administrator permissions do not extend the selection. The responsible person, participants, and observers of a template do not get access to it.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "params": {
        "select": ["ID", "TITLE", "CREATED_BY", "RESPONSIBLE_ID", "ACCOMPLICES", "AUDITORS", "REPLICATE", "GROUP_ID"],
        "filter": {
          "CREATED_BY": 1,
          "REPLICATE": "N"
        },
        "order": {
          "ID": "DESC"
        }
      },
      "start": 0
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "params": {
        "select": ["ID", "TITLE", "CREATED_BY", "RESPONSIBLE_ID", "ACCOMPLICES", "AUDITORS", "REPLICATE", "GROUP_ID"],
        "filter": {
          "CREATED_BY": 1,
          "REPLICATE": "N"
        },
        "order": {
          "ID": "DESC"
        }
      },
      "start": 0,
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type TaskTemplate = {
      ID: number
      TITLE: string
      CREATED_BY: string
      RESPONSIBLE_ID: string
      ACCOMPLICES: number[]
      AUDITORS: number[]
      REPLICATE: string
      GROUP_ID: string | null
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    // An empty page comes as an array [], a non-empty one as an object keyed by template ID
    type TaskTemplateListResult = {
      task_templates: Record<string, TaskTemplate> | []
    }

    try {
      // tasks.template.list returns a single page (50 records). Pass `start` to get the next page.
      const response = await $b24.actions.v2.call.make<TaskTemplateListResult>({
        method: 'tasks.template.list',
        params: {
          params: {
            select: ['ID', 'TITLE', 'CREATED_BY', 'RESPONSIBLE_ID', 'ACCOMPLICES', 'AUDITORS', 'REPLICATE', 'GROUP_ID'],
            filter: {
              CREATED_BY: 1,
              REPLICATE: 'N',
            },
            order: {
              ID: 'DESC',
            },
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const templates = Object.values(response.getData()!.result.task_templates)
        templates.forEach((template) => console.info(template.ID, template.TITLE))
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
      async function getTaskTemplates() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // tasks.template.list returns a single page (50 records). Pass `start` to get the next page.
          const response = await $b24.actions.v2.call.make({
            method: 'tasks.template.list',
            params: {
              params: {
                select: ['ID', 'TITLE', 'CREATED_BY', 'RESPONSIBLE_ID', 'ACCOMPLICES', 'AUDITORS', 'REPLICATE', 'GROUP_ID'],
                filter: {
                  CREATED_BY: 1,
                  REPLICATE: 'N',
                },
                order: {
                  ID: 'DESC',
                },
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          // An empty page comes as an array [], a non-empty one as an object keyed by template ID
          const templates = Object.values(response.getData().result.task_templates)
          templates.forEach((template) => console.info(template.ID, template.TITLE))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTaskTemplates)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        # b24pysdk has no wrapper for tasks.template.list, so the method
        # is called directly via bitrix_token.call_method()
        response = bitrix_token.call_method(
            api_method="tasks.template.list",
            params={
                "params": {
                    "select": ["ID", "TITLE", "CREATED_BY", "RESPONSIBLE_ID", "ACCOMPLICES", "AUDITORS", "REPLICATE", "GROUP_ID"],
                    "filter": {
                        "CREATED_BY": 1,
                        "REPLICATE": "N",
                    },
                    "order": {
                        "ID": "DESC",
                    },
                },
                "start": 0,
            },
        )
        # An empty page comes as an array [], a non-empty one as an object keyed by template ID
        templates = response["result"]["task_templates"] or {}
        for template in templates.values():
            print(template["ID"], template["TITLE"])
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.template.list',
                [
                    'params' => [
                        'select' => ['ID', 'TITLE', 'CREATED_BY', 'RESPONSIBLE_ID', 'ACCOMPLICES', 'AUDITORS', 'REPLICATE', 'GROUP_ID'],
                        'filter' => [
                            'CREATED_BY' => 1,
                            'REPLICATE' => 'N',
                        ],
                        'order' => [
                            'ID' => 'DESC',
                        ],
                    ],
                    'start' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);

    } catch (Throwable $e) {
        echo 'Error retrieving the template list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.template.list',
        {
            params: {
                select: ['ID', 'TITLE', 'CREATED_BY', 'RESPONSIBLE_ID', 'ACCOMPLICES', 'AUDITORS', 'REPLICATE', 'GROUP_ID'],
                filter: {
                    CREATED_BY: 1,
                    REPLICATE: 'N'
                },
                order: {
                    ID: 'DESC'
                }
            },
            start: 0
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
        'tasks.template.list',
        [
            'params' => [
                'select' => ['ID', 'TITLE', 'CREATED_BY', 'RESPONSIBLE_ID', 'ACCOMPLICES', 'AUDITORS', 'REPLICATE', 'GROUP_ID'],
                'filter' => [
                    'CREATED_BY' => 1,
                    'REPLICATE' => 'N',
                ],
                'order' => [
                    'ID' => 'DESC',
                ],
            ],
            'start' => 0,
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.template.list", b24.Params{
    	"params": b24.Params{
    		"select": []string{"ID", "TITLE", "CREATED_BY", "RESPONSIBLE_ID", "ACCOMPLICES", "AUDITORS", "REPLICATE", "GROUP_ID"},
    		"filter": b24.Params{
    			"CREATED_BY": 1,
    			"REPLICATE":  "N",
    		},
    		"order": b24.Params{
    			"ID": "DESC",
    		},
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.list: %w", err)
    }

    // An empty page comes as an array [], a non-empty one as an object keyed by template ID,
    // so unmarshal task_templates into json.RawMessage first
    var page struct {
    	TaskTemplates json.RawMessage `json:"task_templates"`
    }
    if err := json.Unmarshal(res.Result, &page); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }

    templates := map[string]struct {
    	ID    b24.ID `json:"ID"`
    	Title string `json:"TITLE"`
    }{}
    if len(page.TaskTemplates) > 0 && page.TaskTemplates[0] == '{' {
    	if err := json.Unmarshal(page.TaskTemplates, &templates); err != nil {
    		return fmt.Errorf("parse templates: %w", err)
    	}
    }
    for _, template := range templates {
    	fmt.Println(template.ID, template.Title)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "task_templates": {
            "77": {
                "ID": 77,
                "TITLE": "Checklist test template",
                "CREATED_BY": "1",
                "RESPONSIBLE_ID": "1",
                "ACCOMPLICES": [],
                "AUDITORS": [],
                "REPLICATE": "N",
                "GROUP_ID": null
            }
        }
    },
    "total": 1,
    "time": {
        "start": 1795003871,
        "finish": 1795003871.889437,
        "duration": 0.8894369602203369,
        "processing": 0,
        "date_start": "2026-11-18T15:11:11+03:00",
        "date_finish": "2026-11-18T15:11:11+03:00",
        "operating_reset_at": 1795004280,
        "operating": 0.1682889461517334
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **next**
[`integer`](../../data-types.md) | The `start` value for the next page. Returned if there are more records in the selection ||
|| **total**
[`integer`](../../data-types.md) | Total number of templates that match the filter and are available to the user ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **task_templates**
[`object`](../../data-types.md) | Task templates. The key is the template ID, the value is an object with the fields from `select`.

If no fields are passed in `select`, the template object contains all fields, including user fields `UF_*`, `SE_CHECKLIST`, `SE_ACCESS`, `SE_TAG`, and `BASE_TEMPLATE_ID`. The fields are described in [Task Template Fields](./fields.md).

Unlike [tasks.template.get](./tasks-template-get.md), the fields `ACCOMPLICES`, `AUDITORS`, `RESPONSIBLES`, and `DEPENDS_ON` are returned as arrays, and `REPLICATE_PARAMS` as an object.

If there are no templates, an empty array `[]` is returned ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "INVALID_QUERY",
    "error_description": "Invalid template query parameters"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `INVALID_QUERY` | Invalid template query parameters | Possible reasons:
- `select`, `filter`, or `order` contains a field that is not in the list of allowed fields
- `select`, `filter`, or `order` is not passed as an array ||
|| `400` | `100` | Invalid value {x} to match with parameter {params}. Should be value of type array. | The `params` parameter is not passed as an object ||
|| `400` | `140` | Internal server error. | The value `*` is passed in `select` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-add.md)
- [{#T}](./tasks-template-update.md)
- [{#T}](./tasks-template-get.md)
- [{#T}](./tasks-template-delete.md)
- [{#T}](./tasks-template-fields.md)

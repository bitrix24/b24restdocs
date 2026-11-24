# Get a List of Task Templates v 3.0 tasks.template.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `tasks.template.list` method returns a list of task templates available to the current user.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of fields to return. Field names match the keys of the [template object](./fields-rest-v3.md#templatedto). If `select` is not set, the method returns only `id`.

The nested objects `tags`, `replicateParams`, and `userFields` can be specified as a whole or field by field with a dot, for example, `replicateParams.period`. Specify creator fields with a dot, for example, `creator.name`.

Only this method returns the `templateChildrenCount` and `subTemplateIds` fields.

The `permissions`, `crmItemIds`, `checklist`, `requireResult`, `checklistAutonumbering`, and `parent` fields and the related objects `group` and `task` are not available in `select`. Retrieve them with the [tasks.template.get](./tasks-template-get-rest-v3.md) method ||
|| **filter**
[`array`](../../data-types.md) | Filter conditions in the following format:
- `["field", "operator", value]`
- `["field", value]` — the `=` operator, or `in` for an array of values

All conditions are combined with a logical AND. The method does not support groups with `or` logic or the `negative` negation.

Fields available for filtering: `id`, `title`, `creatorId`, `responsibleId`, `groupId`, `taskId`, `baseTemplateId`, `stageId`, `xmlId`, `priority`, `tParamType`, `replicate`, `tags`. For `tags`, pass the tag name as a string.

Operators: `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`, `like`. The `between` operator is not supported. For `in`, pass an array of values.

[More about filtering in REST 3.0](../../rest-v3.md#filter) ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format.

Possible directions:
- `asc` — ascending
- `desc` — descending

Fields available for sorting: `id`, `title`, `groupId`, `taskId`, `baseTemplateId`, `templateChildrenCount`, `stageId`, `tParamType`, `responsibleLastName`, `dependsOn`.

By default, templates are sorted by `id` in ascending order ||
|| **pagination**
[`object`](../../data-types.md) | Pagination parameters:
- `limit` — number of templates per page, greater than `0`. Default: `50`, maximum: `1000`
- `offset` — offset from the beginning of the list, not less than `0`
- `page` — page number. The offset is calculated as `(page - 1) * limit`

The method does not return the total number of templates. If a page has fewer records than `limit`, it is the last page ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","priority","replicate","templateChildrenCount"],"filter":[["title","like","%report%"],["creatorId","=",1269]],"order":{"id":"desc"},"pagination":{"limit":2}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","priority","replicate","templateChildrenCount"],"filter":[["title","like","%report%"],["creatorId","=",1269]],"order":{"id":"desc"},"pagination":{"limit":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.list
    ```

- JS (TS)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TemplateListResult = {
      items: Array<{
        id: number
        title: string
        priority: 'low' | 'average' | 'high'
        replicate: boolean
        templateChildrenCount: number
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateListResult>(
        'tasks.template.list',
        {
          select: ['id', 'title', 'priority', 'replicate', 'templateChildrenCount'],
          filter: [
            ['title', 'like', '%report%'],
            ['creatorId', '=', 1269],
          ],
          order: { id: 'desc' },
          pagination: { limit: 2 },
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(template => console.info(template.id, template.title))
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listTemplates() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.list',
            {
              select: ['id', 'title', 'priority', 'replicate', 'templateChildrenCount'],
              filter: [
                ['title', 'like', '%report%'],
                ['creatorId', '=', 1269],
              ],
              order: { id: 'desc' },
              pagination: { limit: 2 },
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(template => console.info(template.id, template.title))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTemplates)
    </script>
    ```

- Python

    The method name matches the previous version method `tasks.template.list`, and REST 3.0 template methods are not yet included in the list of new version methods in `b24pysdk`: a regular SDK call goes to the previous REST version. Add the method to the `Config().api_v3_methods` list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["id", "title", "priority", "replicate", "templateChildrenCount"],
                "filter": [
                    ["title", "like", "%report%"],
                    ["creatorId", "=", 1269],
                ],
                "order": {"id": "desc"},
                "pagination": {"limit": 2},
            },
            prefer_version=3,
        )
        for template in response["result"]["items"]:
            print(template["id"], template["title"])
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.template.list',
                [
                    'select' => ['id', 'title', 'priority', 'replicate', 'templateChildrenCount'],
                    'filter' => [
                        ['title', 'like', '%report%'],
                        ['creatorId', '=', 1269],
                    ],
                    'order' => ['id' => 'desc'],
                    'pagination' => ['limit' => 2],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'tasks.template.list',
        {
            select: ['id', 'title', 'priority', 'replicate', 'templateChildrenCount'],
            filter: [
                ['title', 'like', '%report%'],
                ['creatorId', '=', 1269],
            ],
            order: { id: 'desc' },
            pagination: { limit: 2 },
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.template.list',
        [
            'select' => ['id', 'title', 'priority', 'replicate', 'templateChildrenCount'],
            'filter' => [
                ['title', 'like', '%report%'],
                ['creatorId', '=', 1269],
            ],
            'order' => ['id' => 'desc'],
            'pagination' => ['limit' => 2],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.list", b24.Params{
    	"select": []string{"id", "title", "priority", "replicate", "templateChildrenCount"},
    	"filter": []any{
    		[]any{"title", "like", "%report%"},
    		[]any{"creatorId", "=", 1269},
    	},
    	"order":      map[string]string{"id": "desc"},
    	"pagination": map[string]int{"limit": 2},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.list: %w", err)
    }

    // The method wraps the list in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("the response has no items key")
    }

    var templates []struct {
    	ID        int    `json:"id"`
    	Title     string `json:"title"`
    	Priority  string `json:"priority"`
    	Replicate bool   `json:"replicate"`
    }
    if err := json.Unmarshal(raw, &templates); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, t := range templates {
    	fmt.Println(t.ID, t.Title)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 153,
                "title": "Monthly sales report",
                "templateChildrenCount": 0,
                "priority": "high",
                "replicate": false
            },
            {
                "id": 151,
                "title": "Daily report",
                "templateChildrenCount": 0,
                "priority": "average",
                "replicate": false
            }
        ]
    },
    "time": {
        "start": 1795429693,
        "finish": 1795429693.652199,
        "duration": 0.6521990299224854,
        "processing": 0,
        "date_start": "2026-11-23T11:28:13+01:00",
        "date_finish": "2026-11-23T11:28:13+01:00",
        "operating_reset_at": 1795429740,
        "operating": 1.2517759799957275
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data ||
|| **items**
[`array`](./fields-rest-v3.md#templatedto) | Array of templates. The set of fields of each template depends on `select`. Fields with the `null` value are not included in the response. [Template field description](./fields-rest-v3.md#templatedto) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION",
        "message": "Incorrect \"pagination\" parameter value: {\"limit\":0}."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `description` | DTO `TemplateDto` in field `description` requires attribute `Filterable` to perform this request | Filter only by the fields listed in the `filter` parameter description ||
|| `priority` | DTO `TemplateDto` in field `priority` requires attribute `Sortable` to perform this request | Sort only by the fields listed in the `order` parameter description ||
|| `id` | This request requires data type `int` in field `id` | In the condition, pass a value of the same type as the field in the [template object](./fields-rest-v3.md#templatedto) ||
|#

#### Unknown Field

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter` | Unknown field `foo` found in entity `TemplateDto` | Specify fields from the [Template Object](./fields-rest-v3.md#templatedto) table ||
|#

#### Unsupported Field or Condition

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_FIELDNOTSUPPORTEDEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | This method does not support `permissions` | Remove the fields unavailable in the list from `select` and retrieve them with the [tasks.template.get](./tasks-template-get-rest-v3.md) method ||
|| `select` | This method does not support `group` | Request the related objects `group` and `task` with the [tasks.template.get](./tasks-template-get-rest-v3.md) method ||
|| `filter` | This method does not support `id` | The error occurs for a condition group with `logic: "or"`, for `negative: true`, and for the `between` operator. Pass a flat list of conditions, and set a range with two conditions, `>=` and `<=` ||
|| `filter` | This method does not support `priority` | For `priority` and `tParamType`, pass a value from the list, for example, `high` or `usual` ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `xx` | Use the `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`, `like` operators ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Operator "in" requires an array value.` | For the `in` operator, pass an array of values, for example, `["id", "in", [1, 3]]` ||
|#

#### Pagination Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass `limit` greater than `0` ||
|| `pagination` | Incorrect `pagination` parameter value: `{"offset":-1}` | Pass `offset` not less than `0` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](./tasks-template-add-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
- [{#T}](./relation/tasks-template-relation-child-list-rest-v3.md)
- [{#T}](./tasks-template-list.md)
- [{#T}](../../rest-v3.md)

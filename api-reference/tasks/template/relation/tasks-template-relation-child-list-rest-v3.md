# Get Child Template List v 3.0 tasks.template.relation.child.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the parent template

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `tasks.template.relation.child.list` returns the child templates of a template with pagination.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Parent template identifier.

The template identifier can be obtained when [creating a template](../tasks-template-add-rest-v3.md) or by using the [template list](../tasks-template-list-rest-v3.md) method ||
|| **select**
[`string[]`](../../../data-types.md) | Child template fields to return. If `select` is not passed, the method returns only `id`.

Possible values:

- `id` — template identifier
- `title` — template title
- `responsibleId` — assignee identifier
- `responsibleCollection` — list of assignees; each element is an object with the `id` key
- `deadlineAfterTs` — number of seconds after a task is created from the template until its deadline
- `matchesWorkTime` — whether working hours are taken into account when calculating deadlines
- `tParamType` — template type
- `subTemplateIds` — identifiers of next-level child templates

For a detailed description of the fields, see the [template object description](../fields-rest-v3.md#templatedto) ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters:

- `page` — page number
- `limit` — number of templates per page, `50` by default, `1000` at most
- `offset` — offset from the beginning of the list. If `page` and `limit` are passed, the offset is calculated automatically ||
|#

{% note info "" %}

The method returns only first-level child templates and does not support the `filter` and `order` parameters: with them, the method returns an error. If `select` contains other fields, include `id` in it — otherwise the template identifier is not returned. The `deadlineAfterTs` field is not included in the response if the template has no deadline set.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.relation.child.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":117,"select":["id","title","responsibleId","deadlineAfterTs","subTemplateIds"],"pagination":{"page":1,"limit":20}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.relation.child.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":117,"select":["id","title","responsibleId","deadlineAfterTs","subTemplateIds"],"pagination":{"page":1,"limit":20},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.relation.child.list
    ```

- JS (TS)

    The `tasks.template.relation.*` methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ChildListResult = {
      items: Array<{
        id: number
        title?: string
        responsibleId?: number
        deadlineAfterTs?: number
        subTemplateIds?: number[]
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ChildListResult>(
        'tasks.template.relation.child.list',
        {
          templateId: 117,
          select: ['id', 'title', 'responsibleId', 'deadlineAfterTs', 'subTemplateIds'],
          pagination: {
            page: 1,
            limit: 20,
          },
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        for (const item of result.items) {
          console.info(item.id, item.title, item.subTemplateIds)
        }
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    The `tasks.template.relation.*` methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getChildTemplates() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.relation.child.list',
            {
              templateId: 117,
              select: ['id', 'title', 'responsibleId', 'deadlineAfterTs', 'subTemplateIds'],
              pagination: {
                page: 1,
                limit: 20,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(item => console.info(item.id, item.title, item.subTemplateIds))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getChildTemplates)
    </script>
    ```

- Python

    The `tasks.template.relation.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `bitrix_token.call_method`, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.relation.child.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 117,
                "select": ["id", "title", "responsibleId", "deadlineAfterTs", "subTemplateIds"],
                "pagination": {
                    "page": 1,
                    "limit": 20,
                },
            },
            prefer_version=3,
        )
        for item in response["result"]["items"]:
            print(item["id"], item.get("title"), item.get("subTemplateIds"))
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
                'tasks.template.relation.child.list',
                [
                    'templateId' => 117,
                    'select' => ['id', 'title', 'responsibleId', 'deadlineAfterTs', 'subTemplateIds'],
                    'pagination' => [
                        'page' => 1,
                        'limit' => 20,
                    ],
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
        'tasks.template.relation.child.list',
        {
            templateId: 117,
            select: ['id', 'title', 'responsibleId', 'deadlineAfterTs', 'subTemplateIds'],
            pagination: {
                page: 1,
                limit: 20,
            },
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
        'tasks.template.relation.child.list',
        [
            'templateId' => 117,
            'select' => ['id', 'title', 'responsibleId', 'deadlineAfterTs', 'subTemplateIds'],
            'pagination' => [
                'page' => 1,
                'limit' => 20,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.relation.child.list", b24.Params{
    	"templateId": 117,
    	"select":     []string{"id", "title", "responsibleId", "deadlineAfterTs", "subTemplateIds"},
    	"pagination": map[string]int{"page": 1, "limit": 20},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.relation.child.list: %w", err)
    }

    var out struct {
    	Items []struct {
    		ID              int    `json:"id"`
    		Title           string `json:"title"`
    		ResponsibleID   int    `json:"responsibleId"`
    		DeadlineAfterTs int    `json:"deadlineAfterTs"`
    		SubTemplateIDs  []int  `json:"subTemplateIds"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range out.Items {
    	fmt.Println(item.ID, item.Title, item.SubTemplateIDs)
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
                "id": 119,
                "title": "Prepare the workplace",
                "responsibleId": 1269,
                "deadlineAfterTs": 86400,
                "subTemplateIds": [
                    121
                ]
            },
            {
                "id": 123,
                "title": "Grant access",
                "responsibleId": 1269,
                "subTemplateIds": []
            }
        ]
    },
    "time": {
        "start": 1795429168,
        "finish": 1795429168.258815,
        "duration": 0.25881505012512207,
        "processing": 0,
        "date_start": "2026-11-23T11:19:28+01:00",
        "date_finish": "2026-11-23T11:19:28+01:00",
        "operating_reset_at": 1795429740,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains the `items` array ||
|| **items**
[`object[]`](../../../data-types.md) | Array of child templates. Each element contains the fields from `select` [(detailed description)](#items). If there are no child templates, the array is empty ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Element of the items Array {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Child template identifier ||
|| **title**
[`string`](../../../data-types.md) | Template title ||
|| **responsibleId**
[`integer`](../../../data-types.md) | Assignee identifier ||
|| **responsibleCollection**
[`object[]`](../../../data-types.md) | List of assignees; each element is an object with the `id` key ||
|| **deadlineAfterTs**
[`integer`](../../../data-types.md) | Number of seconds after a task is created from the template until its deadline. Not returned if no deadline is set ||
|| **matchesWorkTime**
[`boolean`](../../../data-types.md) | Whether working hours are taken into account when calculating deadlines:

- `true` — deadlines are calculated in working hours, excluding weekends and holidays
- `false` — deadlines are calculated in calendar time ||
|| **tParamType**
[`string`](../../../data-types.md) | Template type:

- `usual` — regular template
- `new_users` — template for a new employee ||
|| **subTemplateIds**
[`integer[]`](../../../data-types.md) | Identifiers of next-level child templates. An empty array if there are none ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_FIELDNOTSUPPORTEDEXCEPTION",
        "message": "This method does not support \"filter\"."
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

HTTP status: **400**

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `templateId` | Required field `templateId` is missing | Add `templateId` to the request body ||
|| `templateId` | This request requires data type `int` in field `templateId` | Pass the identifier as a number, not a string ||
|#

#### Parameter Not Supported

HTTP status: **400**

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_FIELDNOTSUPPORTEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter` | This method does not support `filter` | Remove `filter` from the request ||
|| `order` | This method does not support `order` | Remove `order` from the request ||
|| `select` | This method does not support `description` | Keep only the fields listed in the `select` parameter description in `select` ||
|#

#### Pagination Error

HTTP status: **400**

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass `limit` greater than zero, `offset` not less than zero, and `page` as a number ||
|#

#### Template Not Found

HTTP status: **404**

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `templateId` | Record with ID `999999` was not found | Check the identifier using the [tasks.template.get](../tasks-template-get-rest-v3.md) method. The method treats a deleted template as not found ||
|#

#### Access Error

HTTP status: **403**

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to read the template from `templateId` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-relation-child-add-rest-v3.md)
- [{#T}](./tasks-template-relation-child-delete-rest-v3.md)
- [{#T}](./tasks-template-relation-child-ids-rest-v3.md)
- [{#T}](./tasks-template-relation-child-list-by-ids-rest-v3.md)
- [{#T}](../fields-rest-v3.md)

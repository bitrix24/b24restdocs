# Get Task Template Recurrence History v 3.0 tasks.template.history.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the template

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `tasks.template.history.list` method returns the template recurrence history: records of scheduled task creation.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../data-types.md) | Filter condition in the REST 3.0 format: an array of conditions `[field, operator, value]`. For more details on the format, see the [REST 3.0 overview](../../rest-v3.md#filter).

A condition on the `templateId` field with the `=` operator is required, for example, `[["templateId","=",47]]`. The value is a positive number, the task template ID. It can be retrieved with the [tasks.template.list](./tasks-template-list.md) method.

Other fields and operators are not supported in the filter ||
|| **select**
[`array`](../../data-types.md) | List of record fields to return.

Available fields: `id`, `templateId`, `type`, `created`, `message`, `relatedTaskId`, `errors`. The fields are described in the [Object items](#items) section.

By default, all fields are returned ||
|| **pagination**
[`object`](../../data-types.md) | Pagination:

- `limit` — number of records to return, from 1 to 1000. The method replaces a value greater than 1000 with 1000
- `offset` — number of records to skip from the beginning of the selection
- `page` — page number. Use it instead of `offset`: the method calculates the offset based on `limit`

By default, 50 records are returned ||
|#

Records are sorted from newest to oldest. The method does not support the `order` parameter. The total number of records is returned by the [tasks.template.history.count](./tasks-template-history-count-rest-v3.md) method.

The history includes only template recurrence events: scheduled task creation and calculation of the next run time. A template from which no tasks have been created on a schedule yet has an empty history.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.history.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["templateId","=",47]],"select":["id","type","created","message","relatedTaskId","errors"],"pagination":{"limit":3,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.history.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["templateId","=",47]],"select":["id","type","created","message","relatedTaskId","errors"],"pagination":{"limit":3,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.history.list
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
    type TemplateHistoryListResult = {
      items: Array<{
        id: number
        type: 'message' | 'error'
        created: string
        message: string
        relatedTaskId: number | null
        errors: Array<{
          code: string
          message: string
        }>
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateHistoryListResult>(
        'tasks.template.history.list',
        {
          filter: [
            ['templateId', '=', 47],
          ],
          select: ['id', 'type', 'created', 'message', 'relatedTaskId', 'errors'],
          pagination: {
            limit: 3,
            offset: 0,
          },
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.items)
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
      async function listTemplateHistory() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.history.list',
            {
              filter: [
                ['templateId', '=', 47],
              ],
              select: ['id', 'type', 'created', 'message', 'relatedTaskId', 'errors'],
              pagination: {
                limit: 3,
                offset: 0,
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
          console.info(result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTemplateHistory)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet included in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.history.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["templateId", "=", 47],
                ],
                "select": ["id", "type", "created", "message", "relatedTaskId", "errors"],
                "pagination": {
                    "limit": 3,
                    "offset": 0,
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
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
                'tasks.template.history.list',
                [
                    'filter' => [
                        ['templateId', '=', 47],
                    ],
                    'select' => ['id', 'type', 'created', 'message', 'relatedTaskId', 'errors'],
                    'pagination' => [
                        'limit' => 3,
                        'offset' => 0,
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
        'tasks.template.history.list',
        {
            filter: [
                ['templateId', '=', 47],
            ],
            select: ['id', 'type', 'created', 'message', 'relatedTaskId', 'errors'],
            pagination: {
                limit: 3,
                offset: 0,
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
        'tasks.template.history.list',
        [
            'filter' => [
                ['templateId', '=', 47],
            ],
            'select' => ['id', 'type', 'created', 'message', 'relatedTaskId', 'errors'],
            'pagination' => [
                'limit' => 3,
                'offset' => 0,
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
    res, err := client.Core().Call(ctx, "tasks.template.history.list", b24.Params{
    	"filter": []any{
    		[]any{"templateId", "=", 47},
    	},
    	"select": []string{"id", "type", "created", "message", "relatedTaskId", "errors"},
    	"pagination": map[string]any{
    		"limit":  3,
    		"offset": 0,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.history.list: %w", err)
    }

    // The method wraps the response in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("the response has no items key")
    }

    var records []struct {
    	ID            int    `json:"id"`
    	Type          string `json:"type"`
    	Created       string `json:"created"`
    	Message       string `json:"message"`
    	RelatedTaskID *int   `json:"relatedTaskId"`
    	Errors        []struct {
    		Code    string `json:"code"`
    		Message string `json:"message"`
    	} `json:"errors"`
    }
    if err := json.Unmarshal(raw, &records); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(len(records))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 1327,
                "type": "message",
                "created": "2026-10-26T04:00:01+01:00",
                "message": "Next run scheduled at: 11/30/2026 04:00:00 am (UTC +01:00) (in 3023999 seconds).",
                "relatedTaskId": null,
                "errors": []
            },
            {
                "id": 1325,
                "type": "message",
                "created": "2026-10-26T04:00:01+01:00",
                "message": "Recurring task created (#4255).",
                "relatedTaskId": 4255,
                "errors": []
            },
            {
                "id": 1323,
                "type": "message",
                "created": "2026-09-28T04:00:01+01:00",
                "message": "Next run scheduled at: 10/26/2026 04:00:00 am (UTC +01:00) (in 2419199 seconds).",
                "relatedTaskId": null,
                "errors": []
            }
        ]
    },
    "time": {
        "start": 1795429109,
        "finish": 1795429110.004726,
        "duration": 1.0047259330749512,
        "processing": 1,
        "date_start": "2026-11-23T11:18:29+01:00",
        "date_finish": "2026-11-23T11:18:30+01:00",
        "operating_reset_at": 1795429680,
        "operating": 0
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
[`object[]`](../../data-types.md) | Array of history records [(detailed description)](#items). The set of fields depends on `select`. If there are no records, the array is empty ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object items {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | History record ID ||
|| **templateId**
[`integer`](../../data-types.md) | Template ID. Matches the value from `filter` ||
|| **type**
[`string`](../../data-types.md) | Event type.

Possible values:

- `message` — informational message
- `error` — error message ||
|| **created**
[`datetime`](../../data-types.md) | Date and time when the event was written to the history ||
|| **message**
[`string`](../../data-types.md) | Event text, up to 255 characters. Returned in the language in which it was written to the history ||
|| **relatedTaskId**
[`integer`](../../data-types.md) | ID of the task created by the recurrence. If the event does not mention a task, `null` is returned.

Task data can be retrieved with the [tasks.task.get](../tasks-task-get-rest-v3.md) method ||
|| **errors**
[`object[]`](../../data-types.md) | Errors with which the event ended [(detailed description)](#errors). For successful events, the array is empty ||
|#

#### Object errors {#errors}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the event error ||
|| **message**
[`string`](../../data-types.md) | Text of the error with which the event ended ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Field value must not be empty.",
                "field": "filter"
            }
        ]
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
|| `filter` | Field value must not be empty | Pass `filter` with the `["templateId","=",<template ID>]` condition ||
|| `filter` | Required field `filter` is missing | Pass a positive template ID in the `templateId` condition ||
|| `templateId` | This request requires data type `int` in field `templateId` | Pass the template ID as a number, not a string ||
|| `type` | DTO `HistoryRecordDto` in field `type` requires attribute `Filterable` to perform this request | Filter only by `templateId` ||
|| `id` | DTO `HistoryRecordDto` in field `id` requires attribute `Sortable` to perform this request | Remove the `order` parameter: the method returns records from newest to oldest ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.templateId` | Filter for required field `templateId` is required | Use the `=` operator for `templateId`. The `in`, `>`, `<`, and other operators are not allowed ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example, `[["templateId","=",47]]`, not as an object ||
|#

#### Pagination Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass `limit` from 1 to 1000 ||
|| `pagination` | Incorrect `pagination` parameter value: `{"offset":-1}` | Pass a non-negative `offset` ||
|#

#### Errors in the select Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `foo` found in entity `HistoryRecordDto` | Pass fields from the `select` parameter description ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `{"errors":["code"]}` | Pass `select` as an array of strings, for example, `["id","errors"]` ||
|#

#### Template Not Found

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Record with ID `999999` was not found | Check the template ID with the [tasks.template.list](./tasks-template-list.md) method. The method does not find a deleted template either ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to view the template and the new task card is available on Bitrix24 ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-history-count-rest-v3.md)
- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
- [{#T}](../tasks-task-get-rest-v3.md)

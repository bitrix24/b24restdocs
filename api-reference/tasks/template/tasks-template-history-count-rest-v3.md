# Get the Number of Task Template History Records v 3.0 tasks.template.history.count

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

The `tasks.template.history.count` method returns the number of records in the task template recurrence history.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../data-types.md) | Filter condition in the REST 3.0 format: an array of conditions `[field, operator, value]`. For more details on the format, see the [REST 3.0 overview](../../rest-v3.md#filter).

A condition on the `templateId` field with the `=` operator is required, for example, `[["templateId","=",47]]`. The value is a positive number, the task template ID. It can be retrieved with the [tasks.template.list](./tasks-template-list.md) method.

Other fields and operators are not supported in the filter ||
|#

The history records themselves are returned by the [tasks.template.history.list](./tasks-template-history-list-rest-v3.md) method. The count helps calculate the number of pages for its `pagination` parameter.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.history.count`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["templateId","=",47]]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.history.count
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["templateId","=",47]],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.history.count
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
    type TemplateHistoryCountResult = {
      count: number
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateHistoryCountResult>(
        'tasks.template.history.count',
        {
          filter: [
            ['templateId', '=', 47],
          ],
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('History records:', result.count)
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
      async function countTemplateHistory() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.history.count',
            {
              filter: [
                ['templateId', '=', 47],
              ],
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('History records:', result.count)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', countTemplateHistory)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet included in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.history.count"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["templateId", "=", 47],
                ],
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["count"])
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
                'tasks.template.history.count',
                [
                    'filter' => [
                        ['templateId', '=', 47],
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
        'tasks.template.history.count',
        {
            filter: [
                ['templateId', '=', 47],
            ],
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
        'tasks.template.history.count',
        [
            'filter' => [
                ['templateId', '=', 47],
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
    res, err := client.Core().Call(ctx, "tasks.template.history.count", b24.Params{
    	"filter": []any{
    		[]any{"templateId", "=", 47},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.history.count: %w", err)
    }

    var data struct {
    	Count int `json:"count"`
    }
    if err := json.Unmarshal(res.Result, &data); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(data.Count)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "count": 6
    },
    "time": {
        "start": 1795429454,
        "finish": 1795429454.700127,
        "duration": 0.7001268863677979,
        "processing": 0,
        "date_start": "2026-11-23T11:24:14+01:00",
        "date_finish": "2026-11-23T11:24:14+01:00",
        "operating_reset_at": 1795430040,
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
|| **count**
[`integer`](../../data-types.md) | Number of records in the template recurrence history. If there are no records, `0` is returned ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION",
        "message": "Record with ID \"999999\" was not found."
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

- [{#T}](./tasks-template-history-list-rest-v3.md)
- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](./fields-rest-v3.md)

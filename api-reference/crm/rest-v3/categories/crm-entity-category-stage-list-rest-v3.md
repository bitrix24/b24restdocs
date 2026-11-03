# Get CRM Pipeline Stages v 3.0 crm.[type].category.stage.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the items of this pipeline

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.category.stage.list` method returns the stages of a single pipeline in their order. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.category.stage.list` ||
|| SPA | `crm.smartprocess<entityTypeId>.category.stage.list`, for example, `crm.smartprocess1038.category.stage.list` ||
|#

For an SPA, the method name includes its type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

Deals have pipeline stages, and so does an SPA if both pipelines and stages are enabled in its type settings. Leads, contacts, companies, estimates, invoices, and my company have no pipeline stage methods.

The method name is written in lowercase: `crm.smartprocess1038.category.stage.list`, not `crm.smartProcess1038.category.stage.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | The pipeline whose stages you want to retrieve. [Parameter description](#filter) ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters. [Parameter description](#pagination)

By default, the method returns the first 50 stages ||
|#

### The filter Parameter {#filter}

Pass a single equality condition on `categoryId` — an array of three elements without an outer array:

```json
"filter": ["categoryId", "=", 127]
```

A list of conditions `[["categoryId", "=", 127]]`, as in other REST 3.0 methods, does not work here: the method returns a [filter error](#filter-errors). The filter does not accept other fields or operators.

For the default deal pipeline, `categoryId` is `0`.

Pipeline IDs can be retrieved with the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method.

{% note warning "" %}

The method has no `order` and `select` parameters. If you pass them, the method does not return an error: it returns stages with all fields in ascending `sort` order.

{% endnote %}

### The pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of stages per page, from `1` to `100`. A value greater than `100` causes an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit`. Zero returns the first page.

If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of stages to skip from the start of the list. A negative value causes an error.

Default: `0` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.category.stage.list`

{% endnote %}

The examples retrieve the stages of pipeline `127` of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":["categoryId","=",127]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.category.stage.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":["categoryId","=",127],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.category.stage.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageListResult = {
      items: Array<{
        stageId: string
        categoryId: number
        name: string
        color: string
        description: string
        semantics: 'process' | 'success' | 'failure'
        sort: number
        isSystem: boolean
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageListResult>(
        'crm.smartprocess1038.category.stage.list',
        {
          filter: ['categoryId', '=', 127],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const stages = response.getData()!.result.items
        stages.forEach(stage => console.info(stage.stageId, stage.name))
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listStages() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.category.stage.list',
            {
              filter: ['categoryId', '=', 127],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const stages = response.getData().result.items
          stages.forEach(stage => console.info(stage.stageId, stage.name))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listStages)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.category.stage.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": ["categoryId", "=", 127],
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
                'crm.smartprocess1038.category.stage.list',
                [
                    'filter' => ['categoryId', '=', 127],
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
        'crm.smartprocess1038.category.stage.list',
        {
            filter: ['categoryId', '=', 127],
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
        'crm.smartprocess1038.category.stage.list',
        [
            'filter' => ['categoryId', '=', 127],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.category.stage.list", b24.Params{
    	"filter": []any{"categoryId", "=", 127},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.category.stage.list: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("response has no items")
    }

    var stages []struct {
    	StageID   string `json:"stageId"`
    	Name      string `json:"name"`
    	Semantics string `json:"semantics"`
    	Sort      int    `json:"sort"`
    }
    if err := json.Unmarshal(raw, &stages); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, stage := range stages {
    	fmt.Println(stage.StageID, stage.Name)
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
                "stageId": "DT1038_127:NEW",
                "categoryId": 127,
                "name": "Start",
                "color": "#22B9FF",
                "description": "",
                "semantics": "process",
                "sort": 10,
                "isSystem": true
            },
            {
                "stageId": "DT1038_127:PREPARATION",
                "categoryId": 127,
                "name": "Prepare",
                "color": "#88B9FF",
                "description": "",
                "semantics": "process",
                "sort": 20,
                "isSystem": false
            },
            {
                "stageId": "DT1038_127:UC_WUHOJE",
                "categoryId": 127,
                "name": "Contract review",
                "color": "#2FC6F6",
                "description": "",
                "semantics": "process",
                "sort": 25,
                "isSystem": false
            },
            {
                "stageId": "DT1038_127:CLIENT",
                "categoryId": 127,
                "name": "Approval",
                "color": "#10e5fc",
                "description": "",
                "semantics": "process",
                "sort": 30,
                "isSystem": false
            },
            {
                "stageId": "DT1038_127:SUCCESS",
                "categoryId": 127,
                "name": "Success",
                "color": "#00ff00",
                "description": "",
                "semantics": "success",
                "sort": 40,
                "isSystem": true
            },
            {
                "stageId": "DT1038_127:FAIL",
                "categoryId": 127,
                "name": "Failed",
                "color": "#ff0000",
                "description": "",
                "semantics": "failure",
                "sort": 50,
                "isSystem": true
            }
        ]
    },
    "time": {
        "start": 1793715166,
        "finish": 1793715166.535221,
        "duration": 0.5352210998535156,
        "processing": 0,
        "date_start": "2026-11-03T17:12:46+03:00",
        "date_finish": "2026-11-03T17:12:46+03:00",
        "operating_reset_at": 1793715720,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.items**
[`object[]`](../../../data-types.md) | Pipeline stages in ascending `sort` order. The fields are described on the page of the [crm.&lt;type&gt;.category.stage.get](./crm-entity-category-stage-get-rest-v3.md#item) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION",
        "message": "Error validating request object filters.",
        "validation": [
            {
                "message": "Filter for required field \"categoryId\" is required.",
                "field": "filter.categoryId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Pipeline Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | Check `categoryId` with the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method. This object type has no pipeline with this ID, or the user has no permission to read its items ||
|#

#### Filter Errors {#filter-errors}

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.categoryId` | Filter for required field `categoryId` is required | Pass the filter as a single condition `["categoryId", "=", <ID>]` without an outer array and with the `=` operator ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Required field `filter` is missing | Pass `filter` with the pipeline ID ||
|| `name` | DTO `StageDto` in field `name` requires attribute `Filterable` to perform this request | The filter accepts only `categoryId`. Remove other fields from `filter` ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect "pagination" parameter value: `#PAGINATION#` | Pass `pagination` as an object with the `limit`, `page`, and `offset` keys only. `limit` — from `1` to `100`, `offset` — `0` or greater ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartprocess1038.category.stage.list`. Only deals and SPAs have stage methods. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and whether pipelines and stages are enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-category-stage-get-rest-v3.md)
- [{#T}](./crm-entity-category-stage-replace-rest-v3.md)
- [{#T}](./crm-entity-category-list-rest-v3.md)

# Get Estimate Stages in CRM v3.0 crm.quote.stage.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read estimates

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

`crm.quote.stage.list` retrieves all estimate stages in ascending `sort` order: in-progress stages, the successful stage, and failed stages.

Unlike the [crm.&lt;type&gt;.category.stage.list](./crm-entity-category-stage-list-rest-v3.md) methods for deals and SPAs, this method does not require a `categoryId` filter: estimates share a single set of stages across Bitrix24.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../../data-types.md) | The stage fields to return, for example, `["stageId", "name", "sort"]`. Available fields are described on the page for [crm.quote.stage.get](./crm-quote-stage-get-rest-v3.md#item).

If the parameter is omitted, the method returns all fields ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters. [Parameter Description](#pagination)

If the parameter is omitted, the method returns all stages ||
|#

{% note warning "" %}

The method does not accept `filter` or `order`: passing them results in an [Unknown Field Error](#unknown-field). Stages are always ordered by ascending `sort`.

{% endnote %}

### The pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | The number of stages per page, from `1` to `100`. A value outside this range causes an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | The page number, starting from `1`. The offset is calculated as `(page - 1) × limit`. Zero returns the first page.

If `page` is passed, `offset` is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | The number of stages to skip from the beginning of the list. A negative value causes an error.

Default: `0` ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% note info "" %}

Calls to the new API include the `/api/` segment in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.quote.stage.list`

{% endnote %}

The examples retrieve all estimate stages with all fields.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.stage.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.quote.stage.list
    ```

- JS (TS)

    CRM methods are not yet included in the REST 3.0 method list recognized by `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageListResult = {
      items: Array<{
        stageId: string
        name: string
        color: string
        semantics: 'process' | 'success' | 'failure'
        sort: number
        isSystem: boolean
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageListResult>(
        'crm.quote.stage.list',
        {},
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

    CRM methods are not yet included in the REST 3.0 method list recognized by `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listStages() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.quote.stage.list',
            {},
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

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.quote.stage.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={},
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.stage.list',
                []
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.quote.stage.list',
        {},
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.stage.list',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.quote.stage.list", b24.Params{}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.quote.stage.list: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("no items in response")
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
                "stageId": "DRAFT",
                "name": "New",
                "color": "#39A8EF",
                "semantics": "process",
                "sort": 10,
                "isSystem": true
            },
            {
                "stageId": "SENT",
                "name": "Sent to client",
                "color": "#2FC6F6",
                "semantics": "process",
                "sort": 20,
                "isSystem": false
            },
            {
                "stageId": "UC_9AD50Y",
                "name": "Price review",
                "color": "#FFA900",
                "semantics": "process",
                "sort": 29,
                "isSystem": false
            },
            {
                "stageId": "APPROVED",
                "name": "Approved",
                "color": "#7BD500",
                "semantics": "success",
                "sort": 30,
                "isSystem": true
            },
            {
                "stageId": "DECLAINED",
                "name": "Declined",
                "color": "#FF5752",
                "semantics": "failure",
                "sort": 40,
                "isSystem": true
            },
            {
                "stageId": "APOLOGY",
                "name": "Rejection reason analysis",
                "color": "#FF5752",
                "semantics": "failure",
                "sort": 50,
                "isSystem": false
            }
        ]
    },
    "time": {
        "start": 1795772512,
        "finish": 1795772512.382435,
        "duration": 0.3824350833892822,
        "processing": 0,
        "date_start": "2026-11-27T12:41:52+03:00",
        "date_finish": "2026-11-27T12:41:52+03:00",
        "operating_reset_at": 1795773060,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object containing the response data ||
|| **result.items**
[`object[]`](../../../data-types.md) | Estimate stages in ascending `sort` order. For field descriptions, see [crm.quote.stage.get](./crm-quote-stage-get-rest-v3.md#item).

If the user has no permission to read estimates, the method returns an empty array ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field `filter` found in entity `StageDto`."
    }
}
```

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Unknown Field Error {#unknown-field}

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `StageDto` | Remove `filter`, `order`, and all parameters other than `select` and `pagination` from the request. If the error refers to a field from `select`, check its name against the [Stage Fields](./crm-quote-stage-get-rest-v3.md#item) table ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Incorrect "pagination" parameter value: `#PAGINATION#` | Pass `pagination` as an object with only the `limit`, `page`, and `offset` keys. `limit` is from `1` to `100`, and `offset` is `0` or greater ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase — `crm.quote.stage.list` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-quote-stage-get-rest-v3.md)
- [{#T}](./crm-quote-stage-add-rest-v3.md)
- [{#T}](./crm-quote-stage-replace-rest-v3.md)

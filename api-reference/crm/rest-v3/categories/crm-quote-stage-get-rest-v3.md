# Get an Estimate Stage by ID in v3.0 crm.quote.stage.get

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

`crm.quote.stage.get` retrieves the fields of a single estimate stage by its ID.

Unlike the [crm.&lt;type&gt;.category.stage.get](./crm-entity-category-stage-get-rest-v3.md) methods for deals and SPAs, this method searches for a stage in the single set of estimate stages without pipelines and supports the `select` parameter.

## Method Parameters

{% include [Required Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | The stage ID, for example, `UC_9AD50Y`. [ID Format](#stage-id) ||
|| **select**
[`string[]`](../../../data-types.md) | The stage fields to return, for example, `["stageId", "name"]`. Available fields are described in the [Stage Fields](#item) table.

If the parameter is omitted, the method returns all fields ||
|#

### Stage ID {#stage-id}

Estimates have no pipelines, so a stage ID is a code without a pipeline prefix. The stages that Bitrix24 creates with CRM have the codes `DRAFT`, `SENT`, `APPROVED`, `DECLAINED`, and `APOLOGY`. Bitrix24 assigns new stages a code such as `UC_9AD50Y`.

Stage codes are returned by [crm.quote.stage.list](./crm-quote-stage-list-rest-v3.md).

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% note info "" %}

Calls to the new API include the `/api/` segment in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.quote.stage.get`

{% endnote %}

The examples retrieve the `UC_9AD50Y` stage.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"UC_9AD50Y"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.stage.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"UC_9AD50Y","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.quote.stage.get
    ```

- JS (TS)

    CRM methods are not yet included in the REST 3.0 method list recognized by `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageGetResult = {
      item: {
        stageId: string
        name: string
        color: string
        semantics: 'process' | 'success' | 'failure'
        sort: number
        isSystem: boolean
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageGetResult>(
        'crm.quote.stage.get',
        {
          id: 'UC_9AD50Y',
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const stage = response.getData()!.result.item
        console.info(stage.stageId, stage.name, stage.semantics)
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
      async function getStage() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.quote.stage.get',
            {
              id: 'UC_9AD50Y',
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const stage = response.getData().result.item
          console.info(stage.stageId, stage.name, stage.semantics)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getStage)
    </script>
    ```

- Python

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.quote.stage.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": "UC_9AD50Y",
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.stage.get',
                [
                    'id' => 'UC_9AD50Y',
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.quote.stage.get',
        {
            id: 'UC_9AD50Y',
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.stage.get',
        [
            'id' => 'UC_9AD50Y',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.quote.stage.get", b24.Params{
    	"id": "UC_9AD50Y",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.quote.stage.get: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item in response")
    }

    var stage struct {
    	StageID   string `json:"stageId"`
    	Name      string `json:"name"`
    	Semantics string `json:"semantics"`
    	Sort      int    `json:"sort"`
    }
    if err := json.Unmarshal(raw, &stage); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(stage.StageID, stage.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "stageId": "UC_9AD50Y",
            "name": "Terms review",
            "color": "#AABBCC",
            "semantics": "process",
            "sort": 29,
            "isSystem": false
        }
    },
    "time": {
        "start": 1795772511,
        "finish": 1795772511.635882,
        "duration": 0.6358819007873535,
        "processing": 0,
        "date_start": "2026-11-27T12:41:51+03:00",
        "date_finish": "2026-11-27T12:41:51+03:00",
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
|| **result.item**
[`object`](../../../data-types.md) | The stage fields. [Field Descriptions](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

#### Stage Fields {#item}

#|
|| **Name**
`type` | **Description** ||
|| **stageId**
[`string`](../../../data-types.md) | The stage ID. [ID Format](#stage-id) ||
|| **name**
[`string`](../../../data-types.md) | The stage name ||
|| **color**
[`string`](../../../data-types.md) | The stage color in the `#RRGGBB` format. An empty string means no color is set ||
|| **semantics**
[`string`](../../../data-types.md) | The stage group.

Possible values:

- `process` — an in-progress stage
- `success` — the successful stage
- `failure` — a failed stage ||
|| **sort**
[`integer`](../../../data-types.md) | The stage position in the list: stages are ordered by ascending `sort` ||
|| **isSystem**
[`boolean`](../../../data-types.md) | Indicates a system stage. The system estimate stages are the first stage `DRAFT`, the successful stage `APPROVED`, and the main failed stage `DECLAINED`.

Possible values:

- `true` — a system stage: it cannot be deleted or moved to another group
- `false` — a regular stage ||
|#

Estimate stages do not have the `categoryId` and `description` fields available for pipeline stages.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Stage was not found.",
                "field": "STAGE_NOT_FOUND"
            }
        ]
    }
}
```

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Required field `id` is not specified | Pass the stage `id` ||
|| `STAGE_NOT_FOUND` | Stage was not found | Check `id` with [crm.quote.stage.list](./crm-quote-stage-list-rest-v3.md). This error is caused by an empty `id`, a nonexistent stage code, a pipeline stage ID such as `C1:NEW`, or the user lacking permission to read estimates ||
|#

If the stage does not exist, the method returns a request validation error rather than the `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` code. Identify this error by `STAGE_NOT_FOUND` in `field`.

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `StageDto` | Check the fields in `select` against the [Stage Fields](#item) table ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase — `crm.quote.stage.get` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-quote-stage-add-rest-v3.md)
- [{#T}](./crm-quote-stage-update-rest-v3.md)
- [{#T}](./crm-quote-stage-list-rest-v3.md)

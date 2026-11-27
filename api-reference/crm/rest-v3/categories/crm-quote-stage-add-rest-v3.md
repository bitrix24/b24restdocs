# Add an Estimate Stage in CRM v3.0 crm.quote.stage.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with the "Manage stages" permission in a CRM access role

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

`crm.quote.stage.add` adds an estimate stage and returns its fields, including the ID.

Unlike the [crm.&lt;type&gt;.category.stage.add](./crm-entity-category-stage-add-rest-v3.md) methods for deals and SPAs, this method does not accept `categoryId`: estimates have no pipelines and share a single set of stages across Bitrix24.

## Method Parameters

{% include [Required Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | An object containing the new stage fields in the `{"field": "value"}` format. The `name` field is required. [Parameter Description](#fields) ||
|#

### The fields Parameter {#fields}

{% include [Required Parameter Note](../../../../_includes/required.md) %}

Field names are written in camelCase: `name`, `color`, `semantics`.

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | The stage name, up to 100 characters. The method rejects empty strings and strings containing only spaces ||
|| **color**
[`string`](../../../data-types.md) | The stage color in the `#RRGGBB` format, for example, `#39A8EF`. An empty string means no color is set ||
|| **semantics**
[`string`](../../../data-types.md) | The stage group.

Possible values:

- `process` — an in-progress stage
- `success` — the successful stage. A second successful stage cannot be added
- `failure` — a failed stage; requires a `sort` value greater than that of the successful stage

Default: `process` ||
|| **sort**
[`integer`](../../../data-types.md) | The stage position in the list: stages are ordered by ascending `sort`. If `sort` is omitted, an in-progress stage is placed last among in-progress stages, before the successful stage. For a failed stage, `sort` is required ||
|#

The `stageId` and `isSystem` fields are read-only: Bitrix24 sets them. Estimate stages do not have the `categoryId` and `description` fields available for pipeline stages. Passing them results in an [Unknown Field Error](#unknown-field).

### Stage Order {#order}

Bitrix24 checks the stage order: in-progress stages come first, followed by the successful stage, then failed stages. Estimates have one successful stage — the system stage `APPROVED`. The method returns an error with the `OPERATION_NOT_ALLOWED_FOR_STAGE` code in `field` if:

- an in-progress stage has a `sort` value greater than that of the successful stage
- a failed stage has a `sort` value lower than that of the successful stage, or `sort` is omitted
- `semantics: success` is passed

The current stage `sort` values are returned by [crm.quote.stage.list](./crm-quote-stage-list-rest-v3.md).

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% note info "" %}

Calls to the new API include the `/api/` segment in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.quote.stage.add`

{% endnote %}

The examples add the in-progress stage "Terms review". It is placed last before the successful stage.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Terms review","color":"#AABBCC"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.stage.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Terms review","color":"#AABBCC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.quote.stage.add
    ```

- JS (TS)

    CRM methods are not yet included in the REST 3.0 method list recognized by `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageAddResult = {
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
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageAddResult>(
        'crm.quote.stage.add',
        {
          fields: {
            name: 'Terms review',
            color: '#AABBCC',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const stage = response.getData()!.result.item
        console.info('Stage created:', stage.stageId)
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
      async function addStage() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.quote.stage.add',
            {
              fields: {
                name: 'Terms review',
                color: '#AABBCC',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const stage = response.getData().result.item
          console.info('Stage created:', stage.stageId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addStage)
    </script>
    ```

- Python

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.quote.stage.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "name": "Terms review",
                    "color": "#AABBCC",
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.stage.add',
                [
                    'fields' => [
                        'name' => 'Terms review',
                        'color' => '#AABBCC',
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.quote.stage.add',
        {
            fields: {
                name: 'Terms review',
                color: '#AABBCC',
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.stage.add',
        [
            'fields' => [
                'name' => 'Terms review',
                'color' => '#AABBCC',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.quote.stage.add", b24.Params{
    	"fields": b24.Params{
    		"name":  "Terms review",
    		"color": "#AABBCC",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.quote.stage.add: %w", err)
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
        "finish": 1795772511.223143,
        "duration": 0.2231431007385254,
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
[`object`](../../../data-types.md) | The new stage fields. For field descriptions, see [crm.quote.stage.get](./crm-quote-stage-get-rest-v3.md#item). The stage ID is in `stageId` ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
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
                "message": "This action is not available for this stage.",
                "field": "OPERATION_NOT_ALLOWED_FOR_STAGE"
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
|| `fields` | Required field `fields` is not specified | Pass the `fields` object ||
|| `sort` | This request requires data type `int` in field `sort` | Pass `sort` as an integer ||
|| `OPERATION_NOT_ALLOWED_FOR_STAGE` | This action is not available for this stage | Check `semantics` and `sort` against the [Stage Order](#order) ||
|| `FIELD_VALUE_NOT_ALLOWED` | Invalid field value | Pass a name in `name` that contains more than spaces ||
|#

For the `OPERATION_NOT_ALLOWED_FOR_STAGE` and `FIELD_VALUE_NOT_ALLOWED` errors, `field` contains the error code, not a field name.

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `name` | Required field "name" is empty | Pass `name` in `fields` ||
|| `name` | Field value must not be empty | Pass a non-empty string in `name` ||
|| `name` | String must not be longer than 100 characters | Shorten the name to 100 characters ||
|| `color` | Field value does not match regular expression | Pass the color in the `#RRGGBB` format or an empty string ||
|| `semantics` | Field value is not one of the possible values: process, success, failure | Pass one of the values from the field description ||
|| `stageId`, `isSystem` | Field "#FIELD#" is read-only | Remove the field from `fields`: it is read-only ||
|#

#### Unknown Field Error {#unknown-field}

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `StageDto` | Check the field name against the [fields Parameter Table](#fields). Estimate stages do not support the `categoryId` and `description` fields. Names are written in camelCase: `name`, not `NAME` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a CRM administrator or a user with the "Manage stages" permission in a CRM access role ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase — `crm.quote.stage.add` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-quote-stage-update-rest-v3.md)
- [{#T}](./crm-quote-stage-get-rest-v3.md)
- [{#T}](./crm-quote-stage-list-rest-v3.md)
- [{#T}](./crm-quote-stage-delete-rest-v3.md)
- [{#T}](./crm-quote-stage-replace-rest-v3.md)

# Update an Estimate Stage in CRM v3.0 crm.quote.stage.update

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

`crm.quote.stage.update` updates the name, color, group, or position of an estimate stage and returns its fields after the update.

Unlike the [crm.&lt;type&gt;.category.stage.update](./crm-entity-category-stage-update-rest-v3.md) methods for deals and SPAs, this method works with the single set of estimate stages without pipelines and returns the stage fields rather than `true`.

## Method Parameters

{% include [Required Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | The stage ID, for example, `UC_9AD50Y`. [ID Format](#stage-id) ||
|| **fields***
[`object`](../../../data-types.md) | An object containing new field values in the `{"field": "value"}` format. [Parameter Description](#fields) ||
|#

### Stage ID {#stage-id}

Estimates have no pipelines, so a stage ID is a code without a pipeline prefix. The stages that Bitrix24 creates with CRM have the codes `DRAFT`, `SENT`, `APPROVED`, `DECLAINED`, and `APOLOGY`. Bitrix24 assigns new stages a code such as `UC_9AD50Y`.

Stage codes are returned by [crm.quote.stage.list](./crm-quote-stage-list-rest-v3.md).

### The fields Parameter {#fields}

Pass only the fields to update. Fields omitted from `fields` retain their current values. The method accepts an empty `fields` object and returns the stage unchanged.

Field names are written in camelCase: `name`, `color`, `semantics`.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | The stage name, up to 100 characters. The method rejects empty strings and strings containing only spaces ||
|| **color**
[`string`](../../../data-types.md) | The stage color in the `#RRGGBB` format. An empty string removes the color ||
|| **semantics**
[`string`](../../../data-types.md) | The stage group.

Possible values:

- `process` — an in-progress stage
- `success` — the successful stage
- `failure` — a failed stage ||
|| **sort**
[`integer`](../../../data-types.md) | The stage position in the list: stages are ordered by ascending `sort` ||
|#

The method does not accept the `stageId` and `isSystem` fields: the `id` parameter specifies the stage. Estimate stages have no `categoryId` or `description` fields.

All fields of a system stage can be changed except `semantics`.

Bitrix24 checks the stage order: in-progress stages come first, followed by the successful stage, then failed stages. Estimates have one successful stage — the system stage `APPROVED`. An in-progress stage therefore cannot be moved to the `success` group, and a new `sort` value must preserve the group order. For example, to move an in-progress stage after the successful stage, pass `semantics: failure` together with the new `sort` value.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% note info "" %}

Calls to the new API include the `/api/` segment in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.quote.stage.update`

{% endnote %}

The examples update the name and color of the `UC_9AD50Y` stage.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"UC_9AD50Y","fields":{"name":"Price review","color":"#FFA900"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.stage.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"UC_9AD50Y","fields":{"name":"Price review","color":"#FFA900"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.quote.stage.update
    ```

- JS (TS)

    CRM methods are not yet included in the REST 3.0 method list recognized by `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageUpdateResult = {
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
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageUpdateResult>(
        'crm.quote.stage.update',
        {
          id: 'UC_9AD50Y',
          fields: {
            name: 'Price review',
            color: '#FFA900',
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const stage = response.getData()!.result.item
        console.info('Stage updated:', stage.name)
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
      async function updateStage() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.quote.stage.update',
            {
              id: 'UC_9AD50Y',
              fields: {
                name: 'Price review',
                color: '#FFA900',
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const stage = response.getData().result.item
          console.info('Stage updated:', stage.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateStage)
    </script>
    ```

- Python

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.quote.stage.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": "UC_9AD50Y",
                "fields": {
                    "name": "Price review",
                    "color": "#FFA900",
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
                'crm.quote.stage.update',
                [
                    'id' => 'UC_9AD50Y',
                    'fields' => [
                        'name' => 'Price review',
                        'color' => '#FFA900',
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
        'crm.quote.stage.update',
        {
            id: 'UC_9AD50Y',
            fields: {
                name: 'Price review',
                color: '#FFA900',
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
        'crm.quote.stage.update',
        [
            'id' => 'UC_9AD50Y',
            'fields' => [
                'name' => 'Price review',
                'color' => '#FFA900',
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
    res, err := client.Core().Call(ctx, "crm.quote.stage.update", b24.Params{
    	"id":     "UC_9AD50Y",
    	"fields": b24.Params{
    		"name":  "Price review",
    		"color": "#FFA900",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.quote.stage.update: %w", err)
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
            "name": "Price review",
            "color": "#FFA900",
            "semantics": "process",
            "sort": 29,
            "isSystem": false
        }
    },
    "time": {
        "start": 1795772511,
        "finish": 1795772512.000405,
        "duration": 1.0004050731658936,
        "processing": 1,
        "date_start": "2026-11-27T12:41:51+03:00",
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
|| **result.item**
[`object`](../../../data-types.md) | All stage fields after the update. For field descriptions, see [crm.quote.stage.get](./crm-quote-stage-get-rest-v3.md#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Field value does not match regular expression.",
                "field": "color"
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
|| `-` | At least one of the required fields must be not empty: id, filter | Pass a non-empty stage `id` ||
|| `-` | Only one of the following values must be provided: id, filter | Pass a non-empty stage `id`. The method does not support the `filter` parameter ||
|| `STAGE_NOT_FOUND` | Stage was not found | Check `id` with [crm.quote.stage.list](./crm-quote-stage-list-rest-v3.md) ||
|| `fields` | Required field `fields` is not specified | Pass the `fields` object ||
|| `sort` | This request requires data type `int` in field `sort` | Pass `sort` as an integer ||
|| `OPERATION_NOT_ALLOWED_FOR_STAGE` | This action is not available for this stage | A system stage cannot be moved to another group. For other stages, check that `semantics` and `sort` preserve the stage order ||
|| `FIELD_VALUE_NOT_ALLOWED` | Invalid field value | Pass a name in `name` that contains more than spaces ||
|#

If `id` is omitted or empty, the method returns both errors without `field` in a single response. For errors with the codes `STAGE_NOT_FOUND`, `OPERATION_NOT_ALLOWED_FOR_STAGE`, and `FIELD_VALUE_NOT_ALLOWED`, `field` contains the error code, not a field name. If the stage does not exist, the method returns a request validation error rather than the `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` code.

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `name` | Field value must not be empty | Pass a non-empty string in `name` ||
|| `name` | String must not be longer than 100 characters | Shorten the name to 100 characters ||
|| `color` | Field value does not match regular expression | Pass the color in the `#RRGGBB` format or an empty string ||
|| `semantics` | Field value is not one of the possible values: process, success, failure | Pass one of the values from the field description ||
|| `stageId`, `isSystem` | Field "#FIELD#" is read-only | Remove the field from `fields` ||
|#

#### Unknown Field Error

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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase — `crm.quote.stage.update` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-quote-stage-get-rest-v3.md)
- [{#T}](./crm-quote-stage-list-rest-v3.md)
- [{#T}](./crm-quote-stage-replace-rest-v3.md)

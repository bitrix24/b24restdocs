# Add a Stage to a CRM Pipeline v 3.0 crm.[type].category.stage.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with the "Manage stages" permission in a CRM access role. For an SPA of a digital workplace — an administrator of this digital workplace or a user with the "Manage stages" permission in a digital workplace access role. The user must also have permission to read the items of this pipeline

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.category.stage.add` method adds a stage to a pipeline and returns its fields together with the ID. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.category.stage.add` ||
|| SPA | `crm.smartprocess<entityTypeId>.category.stage.add`, for example, `crm.smartprocess1038.category.stage.add` ||
|#

For an SPA, the method name includes its type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

Pipeline stages are available for deals, and for an SPA if pipelines and stages are enabled in its type settings. Leads, contacts, companies, estimates, invoices, and my company have no pipeline stage methods.

The method name is written in lowercase: `crm.smartprocess1038.category.stage.add`, not `crm.smartProcess1038.category.stage.add`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new stage in the format `{"field": "value"}`. The required fields are `categoryId` and `name`. [Parameter description](#fields) ||
|#

### The fields Parameter {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

Field names are written in camelCase: `categoryId`, `name`, `semantics`.

#|
|| **Name**
`type` | **Description** ||
|| **categoryId***
[`integer`](../../../data-types.md) | ID of the pipeline to add the stage to. The default deal pipeline has the ID `0`.

Pipeline IDs can be retrieved with the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method ||
|| **name***
[`string`](../../../data-types.md) | Stage name, up to 100 characters. The method does not accept an empty string or a string of spaces only ||
|| **color**
[`string`](../../../data-types.md) | Stage color in the `#RRGGBB` format, for example, `#39A8EF`. An empty string means no color is set ||
|| **description**
[`string`](../../../data-types.md) | Stage description ||
|| **semantics**
[`string`](../../../data-types.md) | Stage group.

Possible values:

- `process` — an in-progress stage
- `success` — the successful stage. A second successful stage cannot be added
- `failure` — a failed stage; requires a `sort` greater than that of the successful stage

Default: `process` ||
|| **sort**
[`integer`](../../../data-types.md) | Position of the stage in the pipeline: stages follow in ascending `sort` order. If `sort` is not passed, an in-progress stage is placed last among the in-progress stages, before the successful stage. For a failed stage, `sort` must be passed ||
|#

The `stageId` and `isSystem` fields are read-only: they are set by Bitrix24. The [crm.&lt;type&gt;.category.stage.field.list](./index.md#field-methods) method shows which stage fields can be changed.

### Stage Order {#order}

Bitrix24 checks the order of stages in a pipeline: in-progress stages first, then the successful stage, and after it the failed stages. There is only one successful stage in a pipeline: Bitrix24 creates it together with the pipeline. The method returns an error with the `OPERATION_NOT_ALLOWED_FOR_STAGE` code in `field` if:

- an in-progress stage has a `sort` greater than that of the successful stage
- a failed stage has a `sort` less than that of the successful stage, or `sort` is not passed
- `semantics: success` is passed

The current `sort` values of the stages are returned by the [crm.&lt;type&gt;.category.stage.list](./crm-entity-category-stage-list-rest-v3.md) method.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.category.stage.add`

{% endnote %}

The examples add the "Document review" stage to pipeline `127` of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"categoryId":127,"name":"Document review","color":"#39A8EF","sort":25}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.category.stage.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"categoryId":127,"name":"Document review","color":"#39A8EF","sort":25},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.category.stage.add
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageAddResult = {
      item: {
        stageId: string
        categoryId: number
        name: string
        color: string
        description: string
        semantics: 'process' | 'success' | 'failure'
        sort: number
        isSystem: boolean
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageAddResult>(
        'crm.smartprocess1038.category.stage.add',
        {
          fields: {
            categoryId: 127,
            name: 'Document review',
            color: '#39A8EF',
            sort: 25,
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

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addStage() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.category.stage.add',
            {
              fields: {
                categoryId: 127,
                name: 'Document review',
                color: '#39A8EF',
                sort: 25,
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

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.category.stage.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "categoryId": 127,
                    "name": "Document review",
                    "color": "#39A8EF",
                    "sort": 25,
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
                'crm.smartprocess1038.category.stage.add',
                [
                    'fields' => [
                        'categoryId' => 127,
                        'name' => 'Document review',
                        'color' => '#39A8EF',
                        'sort' => 25,
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
        'crm.smartprocess1038.category.stage.add',
        {
            fields: {
                categoryId: 127,
                name: 'Document review',
                color: '#39A8EF',
                sort: 25,
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
        'crm.smartprocess1038.category.stage.add',
        [
            'fields' => [
                'categoryId' => 127,
                'name' => 'Document review',
                'color' => '#39A8EF',
                'sort' => 25,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.category.stage.add", b24.Params{
    	"fields": b24.Params{
    		"categoryId": 127,
    		"name":       "Document review",
    		"color":      "#39A8EF",
    		"sort":       25,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.category.stage.add: %w", err)
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
    fmt.Println(stage.StageID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "stageId": "DT1038_127:UC_WUHOJE",
            "categoryId": 127,
            "name": "Document review",
            "color": "#39A8EF",
            "description": "",
            "semantics": "process",
            "sort": 25,
            "isSystem": false
        }
    },
    "time": {
        "start": 1793715156,
        "finish": 1793715156.236756,
        "duration": 0.2367560863494873,
        "processing": 0,
        "date_start": "2026-11-03T17:12:36+03:00",
        "date_finish": "2026-11-03T17:12:36+03:00",
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
|| **result.item**
[`object`](../../../data-types.md) | Fields of the new stage. The fields are described on the page of the [crm.&lt;type&gt;.category.stage.get](./crm-entity-category-stage-get-rest-v3.md#item) method. The stage ID is in `stageId` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Pipeline Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | Check `categoryId` with the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method. This object type has no pipeline with this ID, or the user has no permission to read its items ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `sort` | This request requires data type `int` in field `sort` | Pass a value of the type specified in the field description ||
|| `OPERATION_NOT_ALLOWED_FOR_STAGE` | This action is not available for this stage | Check `semantics` and `sort` against the [stage order](#order) ||
|| `FIELD_VALUE_NOT_ALLOWED` | Invalid field value | Pass a name in `name` that contains more than spaces ||
|#

For the `OPERATION_NOT_ALLOWED_FOR_STAGE` and `FIELD_VALUE_NOT_ALLOWED` errors, `field` contains the error code, not a field name.

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `categoryId` | Required field "categoryId" is empty | Pass `categoryId` in `fields` ||
|| `name` | Required field "name" is empty | Pass `name` in `fields` ||
|| `name` | Field value must not be empty | Pass a non-empty string in `name` ||
|| `name` | String must not be longer than 100 characters | Shorten the name to 100 characters ||
|| `color` | Field value does not match regular expression | Pass the color in the `#RRGGBB` format or an empty string ||
|| `semantics` | Field value is not one of the possible values: process, success, failure | Pass one of the values from the field description ||
|| `stageId`, `isSystem` | Field "#FIELD#" is read-only | Remove the field from `fields`: it is read-only ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `StageDto` | Check the parameter name against the [fields parameter table](#fields). Names are written in camelCase: `name`, not `NAME` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a CRM administrator or a user with the "Manage stages" permission in an access role ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartprocess1038.category.stage.add`. Only deals and SPAs have stage methods. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and whether pipelines and stages are enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-category-stage-update-rest-v3.md)
- [{#T}](./crm-entity-category-stage-get-rest-v3.md)
- [{#T}](./crm-entity-category-stage-list-rest-v3.md)
- [{#T}](./crm-entity-category-stage-delete-rest-v3.md)
- [{#T}](./crm-entity-category-stage-replace-rest-v3.md)

# Replace CRM Pipeline Stages v 3.0 crm.[type].category.stage.replace

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

The `crm.<type>.category.stage.replace` method sets the entire set of in-progress stages of a pipeline in a single request and returns the pipeline stages after the replacement. The object type is set by the `<type>` segment of the method name.

The method compares the passed set with the pipeline stages by `stageId`:

- a stage with `stageId` is kept, and the method updates its name, and its color if passed
- a stage without `stageId` is created in the `process` group
- an in-progress stage that is not in the set is deleted if it has no items
- the order of stages in the set becomes the order of stages in the pipeline: the set is placed after the first system stage and before the successful stage

The method does not delete or rename system stages and failed stages, so you do not need to pass them in the set. The method renumbers their `sort` values along with those of the other stages. An empty `stages` array deletes all in-progress stages except the first system stage. If at least one change causes an error, the method does not change the pipeline.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.category.stage.replace` ||
|| SPA | `crm.smartprocess<entityTypeId>.category.stage.replace`, for example, `crm.smartprocess1038.category.stage.replace` ||
|#

For an SPA, the method name includes its type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

Deals have pipeline stages, and so does an SPA if both pipelines and stages are enabled in its type settings. Leads, contacts, companies, estimates, invoices, and my company have no pipeline stage methods.

The method name is written in lowercase: `crm.smartprocess1038.category.stage.replace`, not `crm.smartProcess1038.category.stage.replace`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **categoryId***
[`integer`](../../../data-types.md) | Pipeline ID. The default deal pipeline has the ID `0`.

Pipeline IDs can be retrieved with the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method ||
|| **stages***
[`object[]`](../../../data-types.md) | In-progress stages in the required order, up to 50 stages. The required field of each stage is `name`. [Parameter description](#stages) ||
|#

### The stages Parameter {#stages}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **stageId**
[`string`](../../../data-types.md) | ID of an existing pipeline stage, for example, `DT1038_127:PREPARATION`. If not passed, the method creates a new stage.

Stage IDs are returned by the [crm.&lt;type&gt;.category.stage.list](./crm-entity-category-stage-list-rest-v3.md) method ||
|| **name***
[`string`](../../../data-types.md) | Stage name, up to 100 characters ||
|| **color**
[`string`](../../../data-types.md) | Stage color in the `#RRGGBB` format. If not passed, the color of an existing stage does not change. An empty string removes the color ||
|#

The method does not accept other stage fields, such as `semantics` and `sort`: Bitrix24 sets the stage group, and the array order determines the stage position in the pipeline.

A system stage or a failed stage can be passed in the set only with its current name and color: then the method does not change it. A different name or color causes the `OPERATION_NOT_ALLOWED_FOR_STAGE` error. A system stage can be renamed with the [crm.&lt;type&gt;.category.stage.update](./crm-entity-category-stage-update-rest-v3.md) method.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.category.stage.replace`

{% endnote %}

The examples set three in-progress stages for pipeline `127` of an SPA with the type ID `1038`: they rename "Prepare", keep "Contract review", and add a new "Budget approval" stage. The method deletes the "Approval" stage, which is not in the set. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"categoryId":127,"stages":[{"stageId":"DT1038_127:PREPARATION","name":"Request preparation"},{"stageId":"DT1038_127:UC_WUHOJE","name":"Contract review"},{"name":"Budget approval","color":"#FFA900"}]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.category.stage.replace
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"categoryId":127,"stages":[{"stageId":"DT1038_127:PREPARATION","name":"Request preparation"},{"stageId":"DT1038_127:UC_WUHOJE","name":"Contract review"},{"name":"Budget approval","color":"#FFA900"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.category.stage.replace
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageReplaceResult = {
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
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageReplaceResult>(
        'crm.smartprocess1038.category.stage.replace',
        {
          categoryId: 127,
          stages: [
            {
              stageId: 'DT1038_127:PREPARATION',
              name: 'Request preparation',
            },
            {
              stageId: 'DT1038_127:UC_WUHOJE',
              name: 'Contract review',
            },
            {
              name: 'Budget approval',
              color: '#FFA900',
            },
          ],
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
      async function replaceStages() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.category.stage.replace',
            {
              categoryId: 127,
              stages: [
                {
                  stageId: 'DT1038_127:PREPARATION',
                  name: 'Request preparation',
                },
                {
                  stageId: 'DT1038_127:UC_WUHOJE',
                  name: 'Contract review',
                },
                {
                  name: 'Budget approval',
                  color: '#FFA900',
                },
              ],
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

      document.addEventListener('DOMContentLoaded', replaceStages)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.category.stage.replace"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "categoryId": 127,
                "stages": [
                    {
                        "stageId": "DT1038_127:PREPARATION",
                        "name": "Request preparation",
                    },
                    {
                        "stageId": "DT1038_127:UC_WUHOJE",
                        "name": "Contract review",
                    },
                    {
                        "name": "Budget approval",
                        "color": "#FFA900",
                    },
                ],
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
                'crm.smartprocess1038.category.stage.replace',
                [
                    'categoryId' => 127,
                    'stages' => [
                        [
                            'stageId' => 'DT1038_127:PREPARATION',
                            'name' => 'Request preparation',
                        ],
                        [
                            'stageId' => 'DT1038_127:UC_WUHOJE',
                            'name' => 'Contract review',
                        ],
                        [
                            'name' => 'Budget approval',
                            'color' => '#FFA900',
                        ],
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
        'crm.smartprocess1038.category.stage.replace',
        {
            categoryId: 127,
            stages: [
                {
                    stageId: 'DT1038_127:PREPARATION',
                    name: 'Request preparation',
                },
                {
                    stageId: 'DT1038_127:UC_WUHOJE',
                    name: 'Contract review',
                },
                {
                    name: 'Budget approval',
                    color: '#FFA900',
                },
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
        'crm.smartprocess1038.category.stage.replace',
        [
            'categoryId' => 127,
            'stages' => [
                [
                    'stageId' => 'DT1038_127:PREPARATION',
                    'name' => 'Request preparation',
                ],
                [
                    'stageId' => 'DT1038_127:UC_WUHOJE',
                    'name' => 'Contract review',
                ],
                [
                    'name' => 'Budget approval',
                    'color' => '#FFA900',
                ],
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.category.stage.replace", b24.Params{
    	"categoryId": 127,
    	"stages": []b24.Params{
    		{
    			"stageId": "DT1038_127:PREPARATION",
    			"name":    "Request preparation",
    		},
    		{
    			"stageId": "DT1038_127:UC_WUHOJE",
    			"name":    "Contract review",
    		},
    		{
    			"name":  "Budget approval",
    			"color": "#FFA900",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.category.stage.replace: %w", err)
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
                "name": "Request preparation",
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
                "sort": 30,
                "isSystem": false
            },
            {
                "stageId": "DT1038_127:UC_0A4L4I",
                "categoryId": 127,
                "name": "Budget approval",
                "color": "#FFA900",
                "description": "",
                "semantics": "process",
                "sort": 40,
                "isSystem": false
            },
            {
                "stageId": "DT1038_127:SUCCESS",
                "categoryId": 127,
                "name": "Success",
                "color": "#00ff00",
                "description": "",
                "semantics": "success",
                "sort": 50,
                "isSystem": true
            },
            {
                "stageId": "DT1038_127:FAIL",
                "categoryId": 127,
                "name": "Failed",
                "color": "#ff0000",
                "description": "",
                "semantics": "failure",
                "sort": 60,
                "isSystem": true
            }
        ]
    },
    "time": {
        "start": 1793715167,
        "finish": 1793715167.141672,
        "duration": 0.14167189598083496,
        "processing": 0,
        "date_start": "2026-11-03T17:12:47+03:00",
        "date_finish": "2026-11-03T17:12:47+03:00",
        "operating_reset_at": 1793715720,
        "operating": 0.10776114463806152
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
[`object[]`](../../../data-types.md) | All pipeline stages after the replacement in ascending `sort` order, including system stages and failed stages. The fields are described on the page of the [crm.&lt;type&gt;.category.stage.get](./crm-entity-category-stage-get-rest-v3.md#item) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

After the replacement, the method renumbers `sort` of all pipeline stages in steps of `10`.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Move or delete the related items first.",
                "field": "DEPENDENT_ITEMS_EXIST"
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
|| `categoryId` | Required field `categoryId` is missing | Pass `categoryId` ||
|| `stages` | Required field `stages` is missing | Pass the `stages` array ||
|| `stages` | Field value must not exceed 50 | Pass no more than 50 stages ||
|| `DEPENDENT_ITEMS_EXIST` | Move or delete the related items first | A stage that is not in the set has items. Add the stage to the set or move the items to another stage ||
|| `STAGE_NOT_FOUND` | Stage was not found | The pipeline has no stage with the passed `stageId`. Check the IDs with the [crm.&lt;type&gt;.category.stage.list](./crm-entity-category-stage-list-rest-v3.md) method ||
|| `OPERATION_NOT_ALLOWED_FOR_STAGE` | This action is not available for this stage | The set contains a system stage or a failed stage with a new name or color. Pass it with its current values or remove it from the set ||
|| `FIELD_VALUE_NOT_ALLOWED` | Invalid field value | The stage name consists of spaces only. Pass a name in `name` that contains more than spaces ||
|| `stages.0.name` | This request requires data type `string` in field `stages.0.name` | Pass `name` as a string ||
|#

For errors with the `DEPENDENT_ITEMS_EXIST`, `STAGE_NOT_FOUND`, `OPERATION_NOT_ALLOWED_FOR_STAGE`, and `FIELD_VALUE_NOT_ALLOWED` codes, `field` contains the error code, not a field name.

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `stages.1.name` | Required field "name" is empty | Pass `name` for each stage. The stage number in `field` is zero-based ||
|| `stages.0.name` | Field value must not be empty | Pass a non-empty string in `name` ||
|| `stages.0.name` | String must not be longer than 100 characters | Shorten the name to 100 characters ||
|| `stages.1.color` | Field value does not match regular expression | Pass the color in the `#RRGGBB` format or an empty string ||
|#

The method returns object validation errors for all stages of the set in a single response.

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `StageInputDto` | A stage in the set has only `stageId`, `name`, and `color`. Remove `semantics`, `sort`, and other fields. An error with the field name `0` is caused by `stages` passed as an object instead of an array ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartprocess1038.category.stage.replace`. Only deals and SPAs have stage methods. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and whether pipelines and stages are enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-category-stage-list-rest-v3.md)
- [{#T}](./crm-entity-category-stage-add-rest-v3.md)
- [{#T}](./crm-entity-category-stage-update-rest-v3.md)
- [{#T}](./crm-entity-category-stage-delete-rest-v3.md)

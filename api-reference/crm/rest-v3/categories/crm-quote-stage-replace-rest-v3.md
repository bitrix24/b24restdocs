# Replace Estimate Stages in CRM v3.0 crm.quote.stage.replace

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

`crm.quote.stage.replace` sets the entire set of in-progress estimate stages in a single request and returns all estimate stages after replacement.

Unlike the [crm.&lt;type&gt;.category.stage.replace](./crm-entity-category-stage-replace-rest-v3.md) methods for deals and SPAs, this method does not accept `categoryId` and changes the single set of estimate stages across Bitrix24.

{% note warning "" %}

The method deletes in-progress stages missing from the set. Before replacement, retrieve the current stages with [crm.quote.stage.list](./crm-quote-stage-list-rest-v3.md) and pass all in-progress stages to retain in the set.

{% endnote %}

## Method Parameters

{% include [Required Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **stages***
[`object[]`](../../../data-types.md) | In-progress stages in the required order, up to 50 stages. The `name` field is required for each stage. [Parameter Description](#stages) ||
|#

### The stages Parameter {#stages}

{% include [Required Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **stageId**
[`string`](../../../data-types.md) | The ID of an existing estimate stage, for example, `SENT` or `UC_9AD50Y`. If omitted or `null`, the method creates a new stage.

Stage IDs are returned by [crm.quote.stage.list](./crm-quote-stage-list-rest-v3.md). Each `stageId` can appear in the set only once ||
|| **name***
[`string`](../../../data-types.md) | The stage name, up to 100 characters ||
|| **color**
[`string`](../../../data-types.md) | The stage color in the `#RRGGBB` format. If omitted, an existing stage retains its color. An empty string removes the color ||
|#

The method compares the passed set with the current estimate stages by `stageId`:

- a stage with `stageId` is retained, and its name is updated, along with its color if passed
- a stage without `stageId` is created in the `process` group
- an in-progress stage missing from the set is deleted if it has no estimates
- the stage order in the set becomes the order of in-progress stages: the set is placed after the first system stage `DRAFT` and before the successful stage `APPROVED`

The method does not delete or rename system or failed stages, so they do not need to be passed in the set. A system or failed stage can be passed in the set, but only with its current name and color. If they differ, the method returns the `OPERATION_NOT_ALLOWED_FOR_STAGE` error. The `DRAFT` stage remains first even if passed in the middle of the set. A system stage can be renamed with [crm.quote.stage.update](./crm-quote-stage-update-rest-v3.md).

An empty `stages` array deletes all in-progress stages except the first system stage.

If any change causes an error, the method leaves the stages unchanged.

The method does not accept other stage fields, such as `semantics` and `sort`: Bitrix24 sets the stage group, and the array order determines its position.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% note info "" %}

Calls to the new API include the `/api/` segment in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.quote.stage.replace`

{% endnote %}

The examples set three in-progress estimate stages: they retain "Sent to client" and "Price review" and add the new "Signing" stage. The method deletes other in-progress stages.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"stages":[{"stageId":"SENT","name":"Sent to client"},{"stageId":"UC_9AD50Y","name":"Price review"},{"name":"Signing","color":"#FFA900"}]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.stage.replace
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"stages":[{"stageId":"SENT","name":"Sent to client"},{"stageId":"UC_9AD50Y","name":"Price review"},{"name":"Signing","color":"#FFA900"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.quote.stage.replace
    ```

- JS (TS)

    CRM methods are not yet included in the REST 3.0 method list recognized by `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageReplaceResult = {
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
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageReplaceResult>(
        'crm.quote.stage.replace',
        {
          stages: [
            {
              stageId: 'SENT',
              name: 'Sent to client',
            },
            {
              stageId: 'UC_9AD50Y',
              name: 'Price review',
            },
            {
              name: 'Signing',
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

    CRM methods are not yet included in the REST 3.0 method list recognized by `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function replaceStages() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.quote.stage.replace',
            {
              stages: [
                {
                  stageId: 'SENT',
                  name: 'Sent to client',
                },
                {
                  stageId: 'UC_9AD50Y',
                  name: 'Price review',
                },
                {
                  name: 'Signing',
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

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.quote.stage.replace"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "stages": [
                    {
                        "stageId": "SENT",
                        "name": "Sent to client",
                    },
                    {
                        "stageId": "UC_9AD50Y",
                        "name": "Price review",
                    },
                    {
                        "name": "Signing",
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.stage.replace',
                [
                    'stages' => [
                        [
                            'stageId' => 'SENT',
                            'name' => 'Sent to client',
                        ],
                        [
                            'stageId' => 'UC_9AD50Y',
                            'name' => 'Price review',
                        ],
                        [
                            'name' => 'Signing',
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.quote.stage.replace',
        {
            stages: [
                {
                    stageId: 'SENT',
                    name: 'Sent to client',
                },
                {
                    stageId: 'UC_9AD50Y',
                    name: 'Price review',
                },
                {
                    name: 'Signing',
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.stage.replace',
        [
            'stages' => [
                [
                    'stageId' => 'SENT',
                    'name' => 'Sent to client',
                ],
                [
                    'stageId' => 'UC_9AD50Y',
                    'name' => 'Price review',
                ],
                [
                    'name' => 'Signing',
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
    res, err := client.Core().Call(ctx, "crm.quote.stage.replace", b24.Params{
    	"stages": []b24.Params{
    		{
    			"stageId": "SENT",
    			"name":    "Sent to client",
    		},
    		{
    			"stageId": "UC_9AD50Y",
    			"name":    "Price review",
    		},
    		{
    			"name":  "Signing",
    			"color": "#FFA900",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.quote.stage.replace: %w", err)
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
                "sort": 30,
                "isSystem": false
            },
            {
                "stageId": "UC_INDJ22",
                "name": "Signing",
                "color": "#FFA900",
                "semantics": "process",
                "sort": 40,
                "isSystem": false
            },
            {
                "stageId": "APPROVED",
                "name": "Approved",
                "color": "#7BD500",
                "semantics": "success",
                "sort": 50,
                "isSystem": true
            },
            {
                "stageId": "DECLAINED",
                "name": "Declined",
                "color": "#FF5752",
                "semantics": "failure",
                "sort": 60,
                "isSystem": true
            },
            {
                "stageId": "APOLOGY",
                "name": "Rejection reason analysis",
                "color": "#FF5752",
                "semantics": "failure",
                "sort": 70,
                "isSystem": false
            }
        ]
    },
    "time": {
        "start": 1795772512,
        "finish": 1795772512.774333,
        "duration": 0.7743330001831055,
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
[`object[]`](../../../data-types.md) | All estimate stages after replacement in ascending `sort` order, including system and failed stages. For field descriptions, see [crm.quote.stage.get](./crm-quote-stage-get-rest-v3.md#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

After replacement, the method renumbers `sort` for all estimate stages in increments of `10`.

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

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `stages` | Required field `stages` is not specified | Pass the `stages` array ||
|| `stages` | Field value must not exceed 50 | Pass no more than 50 stages ||
|| `stages` | This request requires data type `array` in field `stages` | Pass `stages` as an array rather than an object ||
|| `stages.0` | This request requires data type `StageInputDto` in field `stages.0` | Pass each stage as an object with the `stageId`, `name`, and `color` fields ||
|| `stages.0.name` | This request requires data type `string` in field `stages.0.name` | Pass `name` as a string ||
|| `DEPENDENT_ITEMS_EXIST` | Move or delete the related items first | A stage missing from the set has estimates. Add the stage to the set or move the estimates to another stage ||
|| `STAGE_NOT_FOUND` | Stage was not found | There is no estimate stage with the passed `stageId`. Check the IDs with [crm.quote.stage.list](./crm-quote-stage-list-rest-v3.md) ||
|| `OPERATION_NOT_ALLOWED_FOR_STAGE` | This action is not available for this stage | The set contains a system or failed stage with a new name or color. Pass it with its current values or remove it from the set ||
|| `FIELD_VALUE_NOT_ALLOWED` | Invalid field value | The stage name consists of spaces only. Pass a name in `name` that contains more than spaces ||
|#

The stage number in `field` is zero-based. For errors with the codes `DEPENDENT_ITEMS_EXIST`, `STAGE_NOT_FOUND`, `OPERATION_NOT_ALLOWED_FOR_STAGE`, and `FIELD_VALUE_NOT_ALLOWED`, `field` contains the error code, not a field name.

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `stages.1.name` | Required field "name" is empty | Pass `name` for each stage ||
|| `stages.0.name` | Field value must not be empty | Pass a non-empty string in `name` ||
|| `stages.0.name` | String must not be longer than 100 characters | Shorten the name to 100 characters ||
|| `stages.1.color` | Field value does not match regular expression | Pass the color in the `#RRGGBB` format or an empty string ||
|| `stages.1.stageId` | Invalid field value | This `stageId` is already in the set. Pass each stage only once ||
|#

The method returns object validation errors for all stages in the set in a single response.

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `StageInputDto` | A stage in the set has only `stageId`, `name`, and `color`. Remove `semantics`, `sort`, and other fields ||
|| `-` | Unknown field `#FIELD#` found in entity `StageDto` | The method accepts only the `stages` parameter. Remove `categoryId` and other parameters from the request ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase — `crm.quote.stage.replace` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-quote-stage-add-rest-v3.md)
- [{#T}](./crm-quote-stage-update-rest-v3.md)
- [{#T}](./crm-quote-stage-list-rest-v3.md)
- [{#T}](./crm-quote-stage-delete-rest-v3.md)

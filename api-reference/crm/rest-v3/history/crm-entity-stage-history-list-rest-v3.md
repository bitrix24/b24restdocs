# Get the Stage History of an Item v 3.0 crm.[type].stagehistory.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the item and permission to view history in a CRM access role

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.stagehistory.list` method returns transitions of CRM items through stages and pipelines: each record contains the new stage and the stage from which the item moved.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.stagehistory.list` ||
|| Lead | `crm.lead.stagehistory.list` ||
|| Invoice | `crm.smartinvoice.stagehistory.list` ||
|| SPA | `crm.smartprocess<entityTypeId>.stagehistory.list`, for example, `crm.smartprocess1038.stagehistory.list` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs `entityTypeId` are returned by the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method. The method exists only for an SPA with stages enabled — `isStagesEnabled`. Contacts, companies, estimates, my company, and documents have no such method.

The method name is written in lowercase: `crm.smartinvoice.stagehistory.list`, not `crm.smartInvoice.stageHistory.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions in the form `["field", "operator", "value"]`. A condition on the item `["ownerId","=",<item ID>]` or a `createdTime` period with both boundaries is required, for example, `[["ownerId","=",69]]`. [Parameter description](#filter) ||
|| **select**
[`array`](../../../data-types.md) | List of record fields to return, for example, `["id","entryType","stageId","previousStageId"]`. [Stage history record fields](#item).

Default: all record fields ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. You can sort by the `id` and `createdTime` fields.

Possible direction values:

- `asc` — ascending
- `desc` — descending

Default: `{"createdTime": "asc", "id": "asc"}` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object. [Parameter description](#pagination) ||
|#

### The filter Parameter {#filter}

The filter accepts only these conditions:

#|
|| **Condition** | **Description** ||
|| `["ownerId","=",69]` | Records of a single item. The value is a positive integer ||
|| `["createdTime",">=","2026-11-01T00:00:00+01:00"]` | Start of the period, inclusive ||
|| `["createdTime","<","2026-12-01T00:00:00+01:00"]` | End of the period, exclusive ||
|#

Filter rules:

- a condition on `ownerId` or both `createdTime` period boundaries are required. Without `ownerId`, the method returns transitions of all items of this object type for the period
- the start of the period must be earlier than the end
- the date and time are passed in full, in the `YYYY-MM-DDThh:mm:ss±hh:mm` format. A date without time, for example, `2026-11-01`, causes an error
- each condition can be used no more than once; conditions are combined with AND logic
- other fields, operators, condition groups with `"logic": "or"`, and negation are not supported. There is no filter by stage: select records by `stageId` in your own code

If the item from `ownerId` does not exist, is in the Recycle Bin, or you have no permission to view it, the method returns the ["Item Not Found"](#entity-not-found) error, not an empty list.

### The pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of records per page. Values greater than `1000` are reduced to `1000`. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of records to skip from the beginning of the list.

Default: `0` ||
|#

{% note info "" %}

The response does not contain the total number of records. If a page returns fewer records than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.stagehistory.list`

{% endnote %}

The examples retrieve the stage transitions of an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",69]],"select":["id","createdTime","entryType","stageId","categoryId","previousStageId","previousCategoryId"],"order":{"createdTime":"asc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.stagehistory.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",69]],"select":["id","createdTime","entryType","stageId","categoryId","previousStageId","previousCategoryId"],"order":{"createdTime":"asc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.stagehistory.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageHistoryListResult = {
      items: Array<{
        id: number
        createdTime: string
        entryType: string
        stageId: string
        categoryId: number | null
        previousStageId: string | null
        previousCategoryId: number | null
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageHistoryListResult>(
        'crm.smartprocess1038.stagehistory.list',
        {
          filter: [
            ['ownerId', '=', 69],
          ],
          select: ['id', 'createdTime', 'entryType', 'stageId', 'categoryId', 'previousStageId', 'previousCategoryId'],
          order: {
            createdTime: 'asc',
          },
          pagination: {
            limit: 50,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Stage history:', result.items)
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
      async function listStageHistory() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.stagehistory.list',
            {
              filter: [
                ['ownerId', '=', 69],
              ],
              select: ['id', 'createdTime', 'entryType', 'stageId', 'categoryId', 'previousStageId', 'previousCategoryId'],
              order: {
                createdTime: 'asc',
              },
              pagination: {
                limit: 50,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Stage history:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listStageHistory)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.stagehistory.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["ownerId", "=", 69],
                ],
                "select": ["id", "createdTime", "entryType", "stageId", "categoryId", "previousStageId", "previousCategoryId"],
                "order": {
                    "createdTime": "asc",
                },
                "pagination": {
                    "limit": 50,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.stagehistory.list',
                [
                    'filter' => [
                        ['ownerId', '=', 69],
                    ],
                    'select' => ['id', 'createdTime', 'entryType', 'stageId', 'categoryId', 'previousStageId', 'previousCategoryId'],
                    'order' => [
                        'createdTime' => 'asc',
                    ],
                    'pagination' => [
                        'limit' => 50,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.stagehistory.list',
        {
            filter: [
                ['ownerId', '=', 69],
            ],
            select: ['id', 'createdTime', 'entryType', 'stageId', 'categoryId', 'previousStageId', 'previousCategoryId'],
            order: {
                createdTime: 'asc',
            },
            pagination: {
                limit: 50,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.stagehistory.list',
        [
            'filter' => [
                ['ownerId', '=', 69],
            ],
            'select' => ['id', 'createdTime', 'entryType', 'stageId', 'categoryId', 'previousStageId', 'previousCategoryId'],
            'order' => [
                'createdTime' => 'asc',
            ],
            'pagination' => [
                'limit' => 50,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.stagehistory.list", b24.Params{
    	"filter": []any{
    		[]any{"ownerId", "=", 69},
    	},
    	"select": []string{"id", "createdTime", "entryType", "stageId", "categoryId", "previousStageId", "previousCategoryId"},
    	"order": b24.Params{
    		"createdTime": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.stagehistory.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID                 int     `json:"id"`
    		CreatedTime        string  `json:"createdTime"`
    		EntryType          string  `json:"entryType"`
    		StageID            string  `json:"stageId"`
    		CategoryID         *int    `json:"categoryId"`
    		PreviousStageID    *string `json:"previousStageId"`
    		PreviousCategoryID *int    `json:"previousCategoryId"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.CreatedTime, item.EntryType, item.StageID)
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
                "id": 215,
                "createdTime": "2026-11-23T11:50:53+01:00",
                "entryType": "creation",
                "stageId": "DT1038_69:NEW",
                "categoryId": 69,
                "previousStageId": null,
                "previousCategoryId": null
            },
            {
                "id": 217,
                "createdTime": "2026-11-23T11:51:06+01:00",
                "entryType": "modification",
                "stageId": "DT1038_69:PREPARATION",
                "categoryId": 69,
                "previousStageId": "DT1038_69:NEW",
                "previousCategoryId": 69
            }
        ]
    },
    "time": {
        "start": 1795431071,
        "finish": 1795431072.028404,
        "duration": 1.0284039974212646,
        "processing": 0,
        "date_start": "2026-11-23T11:51:11+01:00",
        "date_finish": "2026-11-23T11:51:12+01:00",
        "operating_reset_at": 1795431660,
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
[`object[]`](../../../data-types.md) | Array of stage history records with the fields from `select`, or with all fields if `select` is not passed. If there are no records, the array is empty. [Stage history record fields](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Stage History Record Fields {#item}

Field descriptions with the `filterable` and `sortable` attributes are returned by the `crm.<type>.stagehistory.field.list` and `crm.<type>.stagehistory.field.get` methods.

#|
|| **Name**
`type` | **Description** | **Filter** | **Sorting** ||
|| **id**
[`integer`](../../../data-types.md) | Stage history record ID | No | Yes ||
|| **ownerId**
[`integer`](../../../data-types.md) | Item ID | Yes | No ||
|| **ownerTypeId**
[`integer`](../../../data-types.md) | Object type ID, for example, `2` for a deal or `1038` for an SPA. The list of types is returned by the [crm.dictionary.entitytype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method | No | No ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Date and time of the transition | Yes | Yes ||
|| **entryType**
[`string`](../../../data-types.md) | Record kind.

Possible values:

- `creation` — the item was created at the `stageId` stage
- `modification` — the item moved to an intermediate stage
- `finalization` — the item moved to a final stage, successful or failed
- `categoryChange` — the item was moved to another pipeline
- `junk` — a service record kind; new records of this kind are not created
- `unknown` — the record kind is not recognized | No | No ||
|| **stageId**
[`string`](../../../data-types.md) | Stage code after the transition, for example, `PREPARATION` for a deal or `DT1038_69:PREPARATION` for an SPA | No | No ||
|| **categoryId**
[`integer`](../../../data-types.md) | Pipeline ID after the transition. For a lead — `null`: leads have no pipelines | No | No ||
|| **previousStageId**
[`string`](../../../data-types.md) | Stage code before the transition. For a `creation` record — `null` | No | No ||
|| **previousCategoryId**
[`integer`](../../../data-types.md) | Pipeline ID before the transition. For a `creation` record and for a lead — `null` | No | No ||
|#

Stages and pipelines of deals and SPAs are returned by the [crm.&lt;type&gt;.category.stage.list](../categories/crm-entity-category-stage-list-rest-v3.md) and [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) methods. Lead and invoice stages are returned by the previous-version method [crm.status.list](../../status/crm-status-list.md).

{% note info "" %}

Stage records of a deleted item are not available. An item restored from the Recycle Bin with the [crm.recyclebin.restore](../recyclebin/crm-recyclebin-restore-rest-v3.md) method gets a new ID, and its stage history starts over.

{% endnote %}

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Invalid filter \"filter.createdTime\".",
                "field": "filter.createdTime"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.createdTime` | Invalid filter `filter.createdTime` | The filter is not passed, it contains neither `ownerId` nor one of the period boundaries, the start of the period is not earlier than the end, or `createdTime` has a different operator. Pass `["ownerId","=",<item ID>]` or both boundaries: `[">=", start]` and `["<", end]` ||
|| `filter.ownerId` | Invalid filter `filter.ownerId` | Pass a single `["ownerId","=",<item ID>]` condition with a positive number ||
|| `filter` | Invalid filter `filter` | Remove condition groups with `"logic": "or"` and negation: conditions are combined only with AND logic ||
|| `createdTime` | This request requires data type `DateTime` in field `createdTime` | Pass the date and time in full, for example, `2026-11-01T00:00:00+01:00` ||
|| `ownerId` | This request requires data type `int` in field `ownerId` | Pass the item ID as a number, not a string ||
|| `stageId` | DTO `StageTransitionDto` in field `stageId` requires attribute `Filterable` to perform this request | Filter only by `ownerId` and `createdTime` ||
|| `stageId` | DTO `StageTransitionDto` in field `stageId` requires attribute `Sortable` to perform this request | Sort only by `id` and `createdTime` ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Item Not Found {#entity-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `9199` was not found | The item from `ownerId` does not exist in this object type, it was deleted, or you have no permission to view it. Check `ownerId` and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `fieldName` found in entity `StageTransitionDto` | Check the field names in `select`, `filter`, and `order` against the [stage history record fields table](#item). The stage history has no filter by `fieldName` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user has no permission to view history in a CRM access role, or, for an SPA of a digital workplace, no permission to view history of this digital workplace. For lead methods — leads are disabled in the CRM settings. Call the method on behalf of a user with the permission or enable leads ||
|#

#### Feature Not Available on the Plan

Error Code: `FEATURE_NOT_AVAILABLE_ON_CURRENT_PLAN`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | This feature is not available on your current plan | The Bitrix24 plan does not include leads. The error occurs only for lead methods ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.contact.stagehistory.list` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.stagehistory.list`. For an SPA, check `entityTypeId` with the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method and whether stages are enabled in the type. Contacts, companies, estimates, my company, and documents have no such method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-history-list-rest-v3.md)
- [{#T}](../categories/crm-entity-category-stage-list-rest-v3.md)
- [{#T}](../../crm-stage-history-list.md)

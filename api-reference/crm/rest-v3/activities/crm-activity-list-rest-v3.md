# Get a List of Activities v 3.0 crm.activity.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a Bitrix24 administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.activity.list` method returns activities of the entire Bitrix24 with filtering, sorting, and pagination. The method is intended for export and synchronization, so it does not check permissions for individual activities and is available only to a Bitrix24 administrator. A CRM administrator without Bitrix24 administrator permissions receives an access error.

The list also includes activities that [crm.activity.get](./crm-activity-get-rest-v3.md) does not return, such as document and call list activities. Activities of a single item for any user with permission for the item are returned by the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`, for example, `[["isCompleted","=",false]]`. To retrieve the activities of a single item, pass an object with a binding. [Parameter description](#filter) ||
|| **select**
[`array`](../../../data-types.md) | List of activity fields to return, for example, `["id","subject","deadlineTime"]`. The available fields are described in the [overview of activity methods](./index.md#fields). For `bindings` and `communications`, you can select nested fields using dot notation: `bindings.entityId`.

Default: all activity fields ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the `{"field": "direction"}` format. You can sort by the `id`, `createdTime`, `updatedTime`, `startTime`, `endTime`, and `deadlineTime` fields.

Possible direction values:

- `asc` — ascending
- `desc` — descending

Default: `{"id": "asc"}` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object. [Parameter description](#pagination) ||
|#

### Parameter filter {#filter}

The filter accepts two formats.

**Array of conditions.** A regular REST 3.0 filter: `[["providerId","=","CRM_TODO"],["isCompleted","=",false]]`. You can filter by `id`, `type`, `providerId`, `providerTypeId`, `isCompleted`, `assignedById`, `direction`, `createdTime`, `updatedTime`, `startTime`, `deadlineTime`. The syntax and operators are described in the [REST 3.0 overview](../../../rest-v3.md#filter).

**Object with a binding.** Activities bound to a single CRM item, with conditions for them:

```json
{
    "binding": {
        "entityTypeId": 1038,
        "entityId": 41
    },
    "conditions": [
        ["isCompleted", "=", false]
    ]
}
```

#|
|| **Name**
`type` | **Description** ||
|| **binding***
[`object`](../../../data-types.md) | CRM item: `entityTypeId` is the object type ID, `entityId` is the item ID. Both values are positive integers ||
|| **conditions**
[`array`](../../../data-types.md) | Conditions in the array format, combined with the binding by AND ||
|#

The object must not contain other keys. The `ownerId` and `ownerTypeId` fields are not supported in the filter: the item is set only via `binding`.

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of activities per page. Values greater than `200` are reduced to `200`. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of activities to skip from the beginning of the list.

Default: `0` ||
|#

{% note info "" %}

The response does not contain the total number of activities. If a page returns fewer activities than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.activity.list`

{% endnote %}

The examples retrieve open activities of the SPA item with the type ID `1038` and the ID `41`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"binding":{"entityTypeId":1038,"entityId":41},"conditions":[["isCompleted","=",false]]},"select":["id","subject","isCompleted","deadlineTime"],"order":{"id":"desc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"binding":{"entityTypeId":1038,"entityId":41},"conditions":[["isCompleted","=",false]]},"select":["id","subject","isCompleted","deadlineTime"],"order":{"id":"desc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.activity.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ActivityListResult = {
      items: Array<{
        id: number
        subject: string
        isCompleted: boolean
        deadlineTime: string | null
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ActivityListResult>(
        'crm.activity.list',
        {
          filter: {
            binding: {
              entityTypeId: 1038,
              entityId: 41,
            },
            conditions: [
              ['isCompleted', '=', false],
            ],
          },
          select: ['id', 'subject', 'isCompleted', 'deadlineTime'],
          order: {
            id: 'desc',
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
        console.info('Activities:', result.items)
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
      async function listActivities() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.activity.list',
            {
              filter: {
                binding: {
                  entityTypeId: 1038,
                  entityId: 41,
                },
                conditions: [
                  ['isCompleted', '=', false],
                ],
              },
              select: ['id', 'subject', 'isCompleted', 'deadlineTime'],
              order: {
                id: 'desc',
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
          console.info('Activities:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listActivities)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.activity.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": {
                    "binding": {
                        "entityTypeId": 1038,
                        "entityId": 41,
                    },
                    "conditions": [
                        ["isCompleted", "=", False],
                    ],
                },
                "select": ["id", "subject", "isCompleted", "deadlineTime"],
                "order": {
                    "id": "desc",
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
                'crm.activity.list',
                [
                    'filter' => [
                        'binding' => [
                            'entityTypeId' => 1038,
                            'entityId' => 41,
                        ],
                        'conditions' => [
                            ['isCompleted', '=', false],
                        ],
                    ],
                    'select' => ['id', 'subject', 'isCompleted', 'deadlineTime'],
                    'order' => [
                        'id' => 'desc',
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
        'crm.activity.list',
        {
            filter: {
                binding: {
                    entityTypeId: 1038,
                    entityId: 41,
                },
                conditions: [
                    ['isCompleted', '=', false],
                ],
            },
            select: ['id', 'subject', 'isCompleted', 'deadlineTime'],
            order: {
                id: 'desc',
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
        'crm.activity.list',
        [
            'filter' => [
                'binding' => [
                    'entityTypeId' => 1038,
                    'entityId' => 41,
                ],
                'conditions' => [
                    ['isCompleted', '=', false],
                ],
            ],
            'select' => ['id', 'subject', 'isCompleted', 'deadlineTime'],
            'order' => [
                'id' => 'desc',
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
    res, err := client.Core().Call(ctx, "crm.activity.list", b24.Params{
    	"filter": b24.Params{
    		"binding": b24.Params{
    			"entityTypeId": 1038,
    			"entityId":     41,
    		},
    		"conditions": []any{
    			[]any{"isCompleted", "=", false},
    		},
    	},
    	"select": []string{"id", "subject", "isCompleted", "deadlineTime"},
    	"order": b24.Params{
    		"id": "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID           int     `json:"id"`
    		Subject      string  `json:"subject"`
    		IsCompleted  bool    `json:"isCompleted"`
    		DeadlineTime *string `json:"deadlineTime"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Subject, item.IsCompleted)
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
                "id": 8463,
                "subject": "Call the client back",
                "isCompleted": false,
                "deadlineTime": "2026-11-20T18:00:00+03:00"
            }
        ]
    },
    "time": {
        "start": 1793711360,
        "finish": 1793711360.764732,
        "duration": 0.7647318840026855,
        "processing": 0,
        "date_start": "2026-11-03T16:09:20+03:00",
        "date_finish": "2026-11-03T16:09:20+03:00",
        "operating_reset_at": 1793711940,
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
[`object[]`](../../../data-types.md) | Array of activities with the fields from `select`; without `select`, with all fields. [Activity field description](./index.md#fields) ||
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
                "message": "crm.activity.list takes no ownerId: narrow it by filter.binding.",
                "field": "ownerId"
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
|| `ownerId` | crm.activity.list takes no ownerId: narrow it by filter.binding. | Remove `ownerId` from the request and pass the item in `filter.binding` ||
|| `binding.entityId` | This request requires data type `positive int` in field `binding.entityId` | Pass `entityTypeId` and `entityId` as positive integers ||
|| `subject` | DTO `ActivityDto` in field `subject` requires attribute `Filterable` to perform this request | Filter only by the fields marked in the "Filter" column of the [field table](./index.md#fields) ||
|| `subject` | DTO `ActivityDto` in field `subject` requires attribute `Sortable` to perform this request | Sort only by `id`, `createdTime`, `updatedTime`, `startTime`, `endTime`, `deadlineTime` ||
|| `type` | This request requires data type `Type` in field `type` | Pass a value from the `type` field list: `meeting`, `call`, `task`, `email`, `activity`, `provider` ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `#FILTER#` | Check the filter format: an object with a binding may contain only the `binding` and `conditions` keys, and `binding` requires both fields. Conditions on `ownerId` and `ownerTypeId` are not supported, nor is `binding` inside an array of conditions ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Access Denied

Error Code: `CRM_ACTIVITY_LIST_ADMIN_ONLY`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied: administrator privileges are required | Call the method on behalf of a Bitrix24 administrator. Activities of a single item without administrator permissions are returned by [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ActivityDto` | Check the field names in `select`, `filter`, and `order` against the [activity field table](./index.md#fields). Fields that depend on the kind of activity, such as provider settings and reminders, are not published in REST 3.0 ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name, for example, `crm.activity.list` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-get-rest-v3.md)
- [{#T}](./crm-entity-activity-list-rest-v3.md)

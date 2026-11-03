# Get a List of Item Activities v 3.0 crm.[type].activity.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the CRM item whose activities are requested

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.activity.list` method returns activities bound to a single CRM item. The object type is set by the `<type>` segment of the method name, and the item by the `ownerId` condition in the filter.

The list includes activities with any binding to the item, and each activity is returned with all its bindings. Document and call list activities are not included in the list.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.activity.list` ||
|| Lead | `crm.lead.activity.list` ||
|| Contact | `crm.contact.activity.list` ||
|| Company | `crm.company.activity.list` ||
|| Estimate | `crm.quote.activity.list` ||
|| Invoice | `crm.smartinvoice.activity.list` ||
|| CRM document | `crm.smartdocument.activity.list` ||
|| SPA | `crm.smartprocess<entityTypeId>.activity.list`, for example, `crm.smartprocess1038.activity.list` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

The method name is written in lowercase: `crm.smartinvoice.activity.list`, not `crm.smartInvoice.activity.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`. The `["ownerId","=",<item ID>]` condition is required, for example, `[["ownerId","=",41],["isCompleted","=",false]]`. [Parameter description](#filter) ||
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

The `ownerId` condition sets the item whose activities you want to retrieve. Rules for it:

- a single condition at the top level of the filter, with the `=` operator and a positive integer
- `ownerId` cannot be specified in a condition group, under negation, with another operator, twice, or outside `filter`
- if the item does not exist, is in the Recycle Bin, or you have no permission to view it, the method returns a "not found" error, not an empty list

The other conditions are combined with it by AND. You can filter by `id`, `type`, `providerId`, `providerTypeId`, `isCompleted`, `assignedById`, `direction`, `createdTime`, `updatedTime`, `startTime`, `deadlineTime`. REST 3.0 operators and condition groups are supported; the syntax is described in the [REST 3.0 overview](../../../rest-v3.md#filter). A condition group is written with the `"type": "filter"` key: `{"type":"filter","logic":"or","conditions":[...]}`.

To find activities without a deadline, use the `["deadlineTime","=",null]` condition.

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

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.activity.list`

{% endnote %}

The examples retrieve open activities of an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",41],["isCompleted","=",false]],"select":["id","subject","isCompleted","deadlineTime"],"order":{"deadlineTime":"asc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.activity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["ownerId","=",41],["isCompleted","=",false]],"select":["id","subject","isCompleted","deadlineTime"],"order":{"deadlineTime":"asc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.activity.list
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
        'crm.smartprocess1038.activity.list',
        {
          filter: [
            ['ownerId', '=', 41],
            ['isCompleted', '=', false],
          ],
          select: ['id', 'subject', 'isCompleted', 'deadlineTime'],
          order: {
            deadlineTime: 'asc',
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
            'crm.smartprocess1038.activity.list',
            {
              filter: [
                ['ownerId', '=', 41],
                ['isCompleted', '=', false],
              ],
              select: ['id', 'subject', 'isCompleted', 'deadlineTime'],
              order: {
                deadlineTime: 'asc',
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

    method = "crm.smartprocess1038.activity.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["ownerId", "=", 41],
                    ["isCompleted", "=", False],
                ],
                "select": ["id", "subject", "isCompleted", "deadlineTime"],
                "order": {
                    "deadlineTime": "asc",
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
                'crm.smartprocess1038.activity.list',
                [
                    'filter' => [
                        ['ownerId', '=', 41],
                        ['isCompleted', '=', false],
                    ],
                    'select' => ['id', 'subject', 'isCompleted', 'deadlineTime'],
                    'order' => [
                        'deadlineTime' => 'asc',
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
        'crm.smartprocess1038.activity.list',
        {
            filter: [
                ['ownerId', '=', 41],
                ['isCompleted', '=', false],
            ],
            select: ['id', 'subject', 'isCompleted', 'deadlineTime'],
            order: {
                deadlineTime: 'asc',
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
        'crm.smartprocess1038.activity.list',
        [
            'filter' => [
                ['ownerId', '=', 41],
                ['isCompleted', '=', false],
            ],
            'select' => ['id', 'subject', 'isCompleted', 'deadlineTime'],
            'order' => [
                'deadlineTime' => 'asc',
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.activity.list", b24.Params{
    	"filter": []any{
    		[]any{"ownerId", "=", 41},
    		[]any{"isCompleted", "=", false},
    	},
    	"select": []string{"id", "subject", "isCompleted", "deadlineTime"},
    	"order": b24.Params{
    		"deadlineTime": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.activity.list: %w", err)
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
[`object[]`](../../../data-types.md) | Array of activities with the fields from `select`; without `select`, with all fields. If the item has no matching activities, the array is empty. [Activity field description](./index.md#fields) ||
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
                "message": "Filter for required field \"ownerId\" is required.",
                "field": "filter.ownerId"
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
|| `filter` | Required field `filter` is missing | Pass `filter` with a condition on `ownerId` ||
|| `ownerId` | ownerId is a condition of the filter: name it as ["ownerId", "=", N] there. | Remove `ownerId` from the request body and pass it as a condition in `filter` ||
|| `subject` | DTO `ActivityDto` in field `subject` requires attribute `Filterable` to perform this request | Filter only by the fields marked in the "Filter" column of the [field table](./index.md#fields) ||
|| `subject` | DTO `ActivityDto` in field `subject` requires attribute `Sortable` to perform this request | Sort only by `id`, `createdTime`, `updatedTime`, `startTime`, `endTime`, `deadlineTime` ||
|| `type` | This request requires data type `Type` in field `type` | Pass a value from the `type` field list: `meeting`, `call`, `task`, `email`, `activity`, `provider` ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.ownerId` | Filter for required field `ownerId` is required | Add the `["ownerId","=",<item ID>]` condition to `filter` ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid filter `#FILTER#` | Check the `ownerId` condition: a single one, at the top level, with the `=` operator. Write condition groups with the `"type": "filter"` key ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | The item from `ownerId` does not exist for this object type, is in the Recycle Bin, or you have no permission to view it. Check `ownerId` and the method name ||
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
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.activity.list`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. My company and B2E documents have no activity methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-activity-get-rest-v3.md)
- [{#T}](./crm-activity-list-rest-v3.md)
- [{#T}](./crm-entity-activity-complete-rest-v3.md)

# Get a List of Deleted CRM Items v 3.0 crm.recyclebin.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user. An administrator sees all CRM Recycle Bin records, other users — only the items they deleted themselves

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.recyclebin.list` method returns CRM Recycle Bin records about deleted deals, leads, contacts, companies, and SPA items.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions in the form `["field", "operator", "value"]`, for example, `[["entityTypeId","=",2]]`. [Parameter description](#filter) ||
|| **select**
[`array`](../../../data-types.md) | List of record fields to return, for example, `["recordId","entityTypeId","id","title"]`. [Recycle Bin record fields](#item).

Default: all record fields ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"field": "direction"}`. You can sort by the `recordId`, `title`, `deletedTime`, and `deletedById` fields.

Possible direction values:

- `asc` — ascending
- `desc` — descending

Default: `{"deletedTime": "desc"}`. Records with the same value of the sort field are ordered by `recordId` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object. [Parameter description](#pagination) ||
|#

### The filter Parameter {#filter}

The filter accepts conditions only on these fields:

#|
|| **Field** | **Operators** | **Example** ||
|| `entityTypeId` | `=`, `in` | `["entityTypeId","in",[2,1038]]` ||
|| `title` | `=`, exact match | `["title","=","Equipment supply"]` ||
|| `deletedById` | `=` | `["deletedById","=",1]` ||
|| `deletedTime` | `>`, `>=`, `<`, `<=`, `between` | `["deletedTime","between",["2026-11-01T00:00:00+01:00","2026-11-30T23:59:59+01:00"]]` ||
|#

Filter rules:

- in `entityTypeId`, you can pass a deal `2`, a lead `1`, a contact `3`, a company `4`, and an SPA type ID. Other types, for example, an invoice `31`, cause an error
- the date and time in `deletedTime` are passed in full, in the `YYYY-MM-DDThh:mm:ss±hh:mm` format
- each field can be used no more than once; conditions are combined with AND logic. To set a period, use `between`
- condition groups with `"logic": "or"` and negation are not supported

There is no filter by `recordId` and `id`. To find the record of a specific item, filter by `entityTypeId` and `title` and check `id` in the response.

### The pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of records per page. Values greater than `100` are reduced to `100`. Zero and negative values cause an error.

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

{% note info "" %}

In cloud Bitrix24, the method returns records for the last 30 days; in the on-premise version, for the retention period set in the CRM Recycle Bin settings. The list includes only object types with the Recycle Bin enabled. If the Recycle Bin is not available on the plan, the method returns an empty list.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.recyclebin.list`

{% endnote %}

The examples retrieve deals and SPA items with the type ID `1038` deleted since November 1, 2026.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["entityTypeId","in",[2,1038]],["deletedTime",">=","2026-11-01T00:00:00+01:00"]],"select":["recordId","entityTypeId","id","title","deletedTime","deletedById"],"order":{"deletedTime":"desc"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.recyclebin.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["entityTypeId","in",[2,1038]],["deletedTime",">=","2026-11-01T00:00:00+01:00"]],"select":["recordId","entityTypeId","id","title","deletedTime","deletedById"],"order":{"deletedTime":"desc"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.recyclebin.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RecyclebinListResult = {
      items: Array<{
        recordId: number
        entityTypeId: number
        id: number
        title: string
        deletedTime: string
        deletedById: number
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<RecyclebinListResult>(
        'crm.recyclebin.list',
        {
          filter: [
            ['entityTypeId', 'in', [2, 1038]],
            ['deletedTime', '>=', '2026-11-01T00:00:00+01:00'],
          ],
          select: ['recordId', 'entityTypeId', 'id', 'title', 'deletedTime', 'deletedById'],
          order: {
            deletedTime: 'desc',
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
        console.info('Deleted items:', result.items)
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
      async function listRecyclebin() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.recyclebin.list',
            {
              filter: [
                ['entityTypeId', 'in', [2, 1038]],
                ['deletedTime', '>=', '2026-11-01T00:00:00+01:00'],
              ],
              select: ['recordId', 'entityTypeId', 'id', 'title', 'deletedTime', 'deletedById'],
              order: {
                deletedTime: 'desc',
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
          console.info('Deleted items:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listRecyclebin)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.recyclebin.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "filter": [
                    ["entityTypeId", "in", [2, 1038]],
                    ["deletedTime", ">=", "2026-11-01T00:00:00+01:00"],
                ],
                "select": ["recordId", "entityTypeId", "id", "title", "deletedTime", "deletedById"],
                "order": {
                    "deletedTime": "desc",
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
                'crm.recyclebin.list',
                [
                    'filter' => [
                        ['entityTypeId', 'in', [2, 1038]],
                        ['deletedTime', '>=', '2026-11-01T00:00:00+01:00'],
                    ],
                    'select' => ['recordId', 'entityTypeId', 'id', 'title', 'deletedTime', 'deletedById'],
                    'order' => [
                        'deletedTime' => 'desc',
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
        'crm.recyclebin.list',
        {
            filter: [
                ['entityTypeId', 'in', [2, 1038]],
                ['deletedTime', '>=', '2026-11-01T00:00:00+01:00'],
            ],
            select: ['recordId', 'entityTypeId', 'id', 'title', 'deletedTime', 'deletedById'],
            order: {
                deletedTime: 'desc',
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
        'crm.recyclebin.list',
        [
            'filter' => [
                ['entityTypeId', 'in', [2, 1038]],
                ['deletedTime', '>=', '2026-11-01T00:00:00+01:00'],
            ],
            'select' => ['recordId', 'entityTypeId', 'id', 'title', 'deletedTime', 'deletedById'],
            'order' => [
                'deletedTime' => 'desc',
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
    res, err := client.Core().Call(ctx, "crm.recyclebin.list", b24.Params{
    	"filter": []any{
    		[]any{"entityTypeId", "in", []int{2, 1038}},
    		[]any{"deletedTime", ">=", "2026-11-01T00:00:00+01:00"},
    	},
    	"select": []string{"recordId", "entityTypeId", "id", "title", "deletedTime", "deletedById"},
    	"order": b24.Params{
    		"deletedTime": "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.recyclebin.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		RecordID     int    `json:"recordId"`
    		EntityTypeID int    `json:"entityTypeId"`
    		ID           int    `json:"id"`
    		Title        string `json:"title"`
    		DeletedTime  string `json:"deletedTime"`
    		DeletedByID  int    `json:"deletedById"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.RecordID, item.EntityTypeID, item.Title)
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
                "recordId": 14533,
                "entityTypeId": 2,
                "id": 9199,
                "title": "Equipment supply",
                "deletedTime": "2026-11-23T11:52:45+01:00",
                "deletedById": 1269
            },
            {
                "recordId": 14463,
                "entityTypeId": 2,
                "id": 9195,
                "title": "Office furniture order",
                "deletedTime": "2026-11-23T11:04:57+01:00",
                "deletedById": 1
            }
        ]
    },
    "time": {
        "start": 1795431217,
        "finish": 1795431217.710354,
        "duration": 0.7103540897369385,
        "processing": 0,
        "date_start": "2026-11-23T11:53:37+01:00",
        "date_finish": "2026-11-23T11:53:37+01:00",
        "operating_reset_at": 1795431780,
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
[`object[]`](../../../data-types.md) | Array of Recycle Bin records with the fields from `select`, or with all fields if `select` is not passed. If there are no records, the array is empty. [Recycle Bin record fields](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Recycle Bin Record Fields {#item}

Field descriptions with the `filterable` and `sortable` attributes are returned by the [crm.recyclebin.field.list](./crm-recyclebin-field-list-rest-v3.md) and [crm.recyclebin.field.get](./crm-recyclebin-field-get-rest-v3.md) methods.

#|
|| **Name**
`type` | **Description** | **Filter** | **Sorting** ||
|| **recordId**
[`integer`](../../../data-types.md) | Recycle Bin record ID. It is passed to the [crm.recyclebin.restore](./crm-recyclebin-restore-rest-v3.md) method | No | Yes ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID: `1` — lead, `2` — deal, `3` — contact, `4` — company, or an SPA type ID, for example, `1038` | Yes | No ||
|| **id**
[`integer`](../../../data-types.md) | Item ID before deletion. It differs from `recordId` | No | No ||
|| **title**
[`string`](../../../data-types.md) | Item title at the time of deletion | Yes | Yes ||
|| **deletedTime**
[`datetime`](../../../data-types.md) | Date and time of deletion | Yes | Yes ||
|| **deletedById**
[`integer`](../../../data-types.md) | ID of the user who deleted the item. User data can be retrieved with the [user.get](../../../user/user-get.md) method | Yes | Yes ||
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
                "message": "Entity type ID is not supported.",
                "field": "filter"
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
|| `filter` | Entity type ID is not supported. | Pass a deal `2`, a lead `1`, a contact `3`, a company `4`, or an SPA type ID in `entityTypeId` ||
|| `filter` | Unsupported recycle bin filter condition. | Check the operator against the [filter table](#filter): `title` and `deletedById` support only `=`, and `entityTypeId` supports `=` and `in` ||
|| `filter` | Unsupported deleted time filter condition. | Use the `>`, `>=`, `<`, `<=`, or `between` operators with two dates for `deletedTime` ||
|| `filter` | Repeated recycle bin filter field. | Specify each field in the filter once. For a `deletedTime` period, use `between` ||
|| `filter` | Only AND filters without negation are supported. | Remove condition groups with `"logic": "or"` and negation ||
|| `filter` | Deleted by ID must be positive. | Pass a positive user ID in `deletedById` ||
|| `deletedTime` | This request requires data type `DateTime` in field `deletedTime` | Pass the date and time in full, for example, `2026-11-01T00:00:00+01:00` ||
|| `id` | DTO `RecyclebinItemDto` in field `id` requires attribute `Filterable` to perform this request | Filter only by `entityTypeId`, `title`, `deletedById`, and `deletedTime` ||
|| `entityTypeId` | DTO `RecyclebinItemDto` in field `entityTypeId` requires attribute `Sortable` to perform this request | Sort only by `recordId`, `title`, `deletedTime`, and `deletedById` ||
|#

#### Unknown Filter Operator

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown filter operator `%` | There is no search by part of the title: `title` is compared only for an exact match with the `=` operator ||
|#

#### Sort Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid sort order `up` | Pass the `asc` or `desc` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass `limit` greater than zero and `offset` not less than zero ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `RecyclebinItemDto` | Check the field names in `select`, `filter`, and `order` against the [Recycle Bin record fields table](#item) ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | REST 3.0 CRM Recycle Bin methods are not available on this Bitrix24, or the call was made without an authorized user ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-recyclebin-restore-rest-v3.md)
- [{#T}](./crm-recyclebin-field-list-rest-v3.md)
- [{#T}](../items/crm-entity-delete-rest-v3.md)

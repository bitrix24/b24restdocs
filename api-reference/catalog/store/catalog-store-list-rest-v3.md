# Get a List of Warehouses v 3.0 catalog.store.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View and select warehouses" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.store.list` method returns a list of warehouses, for example, all active warehouses or only pickup points.

The list includes the warehouses available to the user through the "View and select warehouses" access permission. A user with the "Edit warehouses" access permission also receives the warehouses they created.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | Array of warehouse fields to return in the response.

Available fields:

- `id` — warehouse identifier
- `title` — warehouse name
- `active` — warehouse activity
- `address` — warehouse address
- `description` — warehouse description
- `gpsN` — warehouse latitude
- `gpsS` — warehouse longitude
- `imageId` — warehouse image file identifier
- `imageUrl` — link to the warehouse image
- `dateModify` — date the warehouse was last modified
- `dateCreate` — warehouse creation date
- `userId` — identifier of the user who created the warehouse
- `modifiedBy` — identifier of the user who last modified the warehouse
- `phone` — warehouse phone number
- `schedule` — warehouse working hours
- `xmlId` — warehouse external ID
- `sort` — warehouse sort order in lists
- `email` — warehouse email
- `issuingCenter` — pickup point flag
- `code` — warehouse symbolic code

The `id` identifier is always returned. The value `["*"]` is not supported.

Default: all fields ||
|| **filter**
[`array`](../../data-types.md) | List filter in the REST 3.0 format: an array of conditions like `["field", "operator", "value"]`, for example `[["active", "=", true]]`. The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter).

You can filter by all fields except `image`, `imageId`, and `imageUrl`. For `active` and `issuingCenter`, pass `true` or `false`; pass dates in the `2026-11-01T00:00:00+02:00` format.

Operators: `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`, and `between`. The filter does not accept the `like` operator, groups with `or` logic, or negation. If you pass an empty list in `in`, the method returns an empty list ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format, for example `{"sort": "asc"}`.

You can sort by all fields except `image`, `imageId`, and `imageUrl`.

Possible direction values:

- `asc` — ascending
- `desc` — descending ||
|| **pagination**
[`object`](../../data-types.md) | Pagination parameters. [Object structure description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of warehouses per page. Values greater than `1000` are reduced to `1000`. Zero, negative, and non-numeric values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. The method skips `(page - 1) * limit` records and ignores `offset` ||
|| **offset**
[`integer`](../../data-types.md) | Number of records to skip from the beginning of the list.

Default: `0` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.store.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","address","active","issuingCenter"],"filter":[["active","=",true],["issuingCenter","=",true]],"order":{"sort":"asc","id":"desc"},"pagination":{"limit":3}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.store.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","address","active","issuingCenter"],"filter":[["active","=",true],["issuingCenter","=",true]],"order":{"sort":"asc","id":"desc"},"pagination":{"limit":3},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.store.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StoreListResult = {
      items: {
        id: number
        title: string
        address: string
        active: boolean
        issuingCenter: boolean
      }[]
    }

    try {
      const response = await $b24.actions.v3.call.make<StoreListResult>({
        method: 'catalog.store.list',
        params: {
          select: ['id', 'title', 'address', 'active', 'issuingCenter'],
          filter: [
            ['active', '=', true],
            ['issuingCenter', '=', true],
          ],
          order: {
            sort: 'asc',
            id: 'desc',
          },
          pagination: {
            limit: 3,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(store => console.info(store.id, store.title))
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listStores() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.store.list',
            params: {
              select: ['id', 'title', 'address', 'active', 'issuingCenter'],
              filter: [
                ['active', '=', true],
                ['issuingCenter', '=', true],
              ],
              order: {
                sort: 'asc',
                id: 'desc',
              },
              pagination: {
                limit: 3,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(store => console.info(store.id, store.title))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listStores)
    </script>
    ```

- Python

    The `catalog.store.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.store.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["id", "title", "address", "active", "issuingCenter"],
                "filter": [
                    ["active", "=", True],
                    ["issuingCenter", "=", True],
                ],
                "order": {
                    "sort": "asc",
                    "id": "desc",
                },
                "pagination": {
                    "limit": 3,
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
                'catalog.store.list',
                [
                    'select' => ['id', 'title', 'address', 'active', 'issuingCenter'],
                    'filter' => [
                        ['active', '=', true],
                        ['issuingCenter', '=', true],
                    ],
                    'order' => [
                        'sort' => 'asc',
                        'id' => 'desc',
                    ],
                    'pagination' => [
                        'limit' => 3,
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
        'catalog.store.list',
        {
            select: ['id', 'title', 'address', 'active', 'issuingCenter'],
            filter: [
                ['active', '=', true],
                ['issuingCenter', '=', true],
            ],
            order: {
                sort: 'asc',
                id: 'desc',
            },
            pagination: {
                limit: 3,
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
        'catalog.store.list',
        [
            'select' => ['id', 'title', 'address', 'active', 'issuingCenter'],
            'filter' => [
                ['active', '=', true],
                ['issuingCenter', '=', true],
            ],
            'order' => [
                'sort' => 'asc',
                'id' => 'desc',
            ],
            'pagination' => [
                'limit' => 3,
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
    res, err := client.Core().Call(ctx, "catalog.store.list", b24.Params{
    	"select": []string{"id", "title", "address", "active", "issuingCenter"},
    	"filter": []any{
    		[]any{"active", "=", true},
    		[]any{"issuingCenter", "=", true},
    	},
    	"order": b24.Params{
    		"sort": "asc",
    		"id":   "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 3,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.store.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID            int    `json:"id"`
    		Title         string `json:"title"`
    		Address       string `json:"address"`
    		Active        bool   `json:"active"`
    		IssuingCenter bool   `json:"issuingCenter"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, store := range result.Items {
    	fmt.Println(store.ID, store.Title)
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
                "id": 13,
                "title": "Warehouse 3",
                "active": true,
                "address": "Warehouse 3",
                "issuingCenter": true
            },
            {
                "id": 11,
                "title": "Warehouse 2",
                "active": true,
                "address": "Torstraße 40, 10119 Berlin, Germany",
                "issuingCenter": true
            },
            {
                "id": 9,
                "title": "AliExpress",
                "active": true,
                "address": "AliExpress",
                "issuingCenter": true
            }
        ]
    },
    "time": {
        "start": 1794922633,
        "finish": 1794922633.749759,
        "duration": 0.7497589588165283,
        "processing": 0,
        "date_start": "2026-11-17T16:37:13+02:00",
        "date_finish": "2026-11-17T16:37:13+02:00",
        "operating_reset_at": 1794923220,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | Array of warehouses on the page. The set of fields depends on `select`. [Field description](#items) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

The method does not return the total number of records. If the response contains fewer records than `limit`, the page is the last one.

#### Items Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Warehouse identifier ||
|| **title**
[`string`](../../data-types.md) | Warehouse name. An empty string if the name is not set ||
|| **active**
[`boolean`](../../data-types.md) | Warehouse activity: `true` — the warehouse is active, `false` — inactive ||
|| **address**
[`string`](../../data-types.md) | Warehouse address ||
|| **description**
[`string`](../../data-types.md) | Warehouse description. An empty string if the description is not set ||
|| **gpsN**
[`double`](../../data-types.md) | Warehouse latitude. If the coordinate is not set, `0` is returned. If Bitrix24 stores a non-numeric value, such as an empty string, `null` is returned ||
|| **gpsS**
[`double`](../../data-types.md) | Warehouse longitude. If the coordinate is not set, `0` is returned. If Bitrix24 stores a non-numeric value, such as an empty string, `null` is returned ||
|| **imageId**
[`integer`](../../data-types.md) | Warehouse image file identifier. `0` if there is no image ||
|| **imageUrl**
[`string`](../../data-types.md) | Link to the warehouse image. An empty string if there is no image ||
|| **dateModify**
[`datetime`](../../data-types.md) | Date the warehouse was last modified ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Warehouse creation date. Can be `null` for warehouses that Bitrix24 created itself ||
|| **userId**
[`integer`](../../data-types.md) | Identifier of the user who created the warehouse. `0` for warehouses that Bitrix24 created itself ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the warehouse ||
|| **phone**
[`string`](../../data-types.md) | Warehouse phone number. An empty string if the phone number is not set ||
|| **schedule**
[`string`](../../data-types.md) | Warehouse working hours. An empty string if the working hours are not set ||
|| **xmlId**
[`string`](../../data-types.md) | Warehouse external ID. An empty string if the ID is not set ||
|| **sort**
[`integer`](../../data-types.md) | Warehouse sort order in lists ||
|| **email**
[`string`](../../data-types.md) | Warehouse email. An empty string if the address is not set ||
|| **issuingCenter**
[`boolean`](../../data-types.md) | Pickup point flag: `true` — the warehouse operates as a pickup point, `false` — a regular warehouse ||
|| **code**
[`string`](../../data-types.md) | Warehouse symbolic code. An empty string if the code is not set ||
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
                "message": "Filter OR groups are not supported"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Filter OR groups are not supported | Remove the group with `or` logic from the filter. Make several requests or use the `in` operator ||
|| `-` | Filter NOT groups are not supported | Remove the negation from the filter. Use the `!=` operator ||
|| `image`, `imageId`, `imageUrl` | DTO `StoreDto` in field `#FIELD#` requires attribute `Filterable` to perform this request | Remove the field from `filter`: you cannot filter by image ||
|| `image`, `imageId`, `imageUrl` | DTO `StoreDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Remove the field from `order`: you cannot sort by image ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `StoreDto` | Specify only the fields from the [items element description](#items) ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["active","=",true]]` ||
|#

#### Filter Operator Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`, `between` operators ||
|#

#### Sorting Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `#ORDER#` | Pass the `asc` or `desc` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass `limit` as a positive number, `offset` as a non-negative number, and `page` as a number ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have the "View product catalog" or "View and select warehouses" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-store-add-rest-v3.md)
- [{#T}](./catalog-store-update-rest-v3.md)
- [{#T}](./catalog-store-get-rest-v3.md)
- [{#T}](./catalog-store-delete-rest-v3.md)
- [{#T}](./catalog-store-field-list.md)

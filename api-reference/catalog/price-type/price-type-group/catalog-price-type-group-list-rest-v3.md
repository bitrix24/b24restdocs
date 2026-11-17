# Get a List of Price Type Bindings to Customer Groups v 3.0 catalog.pricetype.group.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "Manage price types" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `catalog.pricetype.group.list` method returns price type bindings to customer groups by filter. A binding defines whether a group can purchase at a price type or only view it. You need the binding identifier from the response to delete the binding with the [catalog.pricetype.group.delete](./catalog-price-type-group-delete-rest-v3.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | Array of binding fields to return in the response.

Available fields:

- `id` — binding identifier
- `catalogGroupId` — price type identifier
- `groupId` — user group identifier
- `access` — price access type

The `id` field is always returned, even if it is not in `select`. The value `["*"]` is not supported.

Default: all fields ||
|| **filter**
[`array`](../../../data-types.md) | Filter in the REST 3.0 format: an array of conditions like `["field", "operator", "value"]`. You can filter by all fields from `select`.

For `access`, pass `true` or `false`. The `Y` and `N` strings cause an error.

The syntax and operators are described in the [REST 3.0 overview](../../../rest-v3.md#filter) ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the `{"field": "direction"}` format. You can sort by all fields from `select`.

Possible direction values:

- `ASC` — ascending
- `DESC` — descending

Default: `{"id": "ASC"}` ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object. [Object structure description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of bindings per page. Values greater than `1000` are reduced to `1000`. Zero and negative values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit`. If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of bindings to skip from the beginning of the list.

Default: `0` ||
|#

{% note info "" %}

The response does not contain the total number of bindings. If a page returns fewer bindings than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.pricetype.group.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["catalogGroupId","groupId","access"],"filter":[["catalogGroupId","=",5]],"order":{"groupId":"asc"},"pagination":{"limit":10}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.pricetype.group.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["catalogGroupId","groupId","access"],"filter":[["catalogGroupId","=",5]],"order":{"groupId":"asc"},"pagination":{"limit":10},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.pricetype.group.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PriceTypeGroupListResult = {
      items: Array<{
        id: number
        catalogGroupId: number
        groupId: number
        access: boolean
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<PriceTypeGroupListResult>({
        method: 'catalog.pricetype.group.list',
        params: {
          select: ['catalogGroupId', 'groupId', 'access'],
          filter: [
            ['catalogGroupId', '=', 5],
          ],
          order: {
            groupId: 'asc',
          },
          pagination: {
            limit: 10,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Bindings:', result.items.length)
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
      async function listPriceTypeGroups() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.pricetype.group.list',
            params: {
              select: ['catalogGroupId', 'groupId', 'access'],
              filter: [
                ['catalogGroupId', '=', 5],
              ],
              order: {
                groupId: 'asc',
              },
              pagination: {
                limit: 10,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Bindings:', result.items.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listPriceTypeGroups)
    </script>
    ```

- Python

    `b24pysdk` has no wrapper for this method, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.pricetype.group.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["catalogGroupId", "groupId", "access"],
                "filter": [
                    ["catalogGroupId", "=", 5],
                ],
                "order": {"groupId": "asc"},
                "pagination": {"limit": 10},
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
                'catalog.pricetype.group.list',
                [
                    'select' => ['catalogGroupId', 'groupId', 'access'],
                    'filter' => [
                        ['catalogGroupId', '=', 5],
                    ],
                    'order' => [
                        'groupId' => 'asc',
                    ],
                    'pagination' => [
                        'limit' => 10,
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
        'catalog.pricetype.group.list',
        {
            select: ['catalogGroupId', 'groupId', 'access'],
            filter: [
                ['catalogGroupId', '=', 5],
            ],
            order: {
                groupId: 'asc',
            },
            pagination: {
                limit: 10,
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
        'catalog.pricetype.group.list',
        [
            'select' => ['catalogGroupId', 'groupId', 'access'],
            'filter' => [
                ['catalogGroupId', '=', 5],
            ],
            'order' => [
                'groupId' => 'asc',
            ],
            'pagination' => [
                'limit' => 10,
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
    res, err := client.Core().Call(ctx, "catalog.pricetype.group.list", b24.Params{
    	"select": []string{"catalogGroupId", "groupId", "access"},
    	"filter": []any{
    		[]any{"catalogGroupId", "=", 5},
    	},
    	"order": b24.Params{
    		"groupId": "asc",
    	},
    	"pagination": b24.Params{
    		"limit": 10,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.pricetype.group.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID             int  `json:"id"`
    		CatalogGroupID int  `json:"catalogGroupId"`
    		GroupID        int  `json:"groupId"`
    		Access         bool `json:"access"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.GroupID, item.Access)
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
                "id": 85,
                "catalogGroupId": 5,
                "groupId": 1,
                "access": false
            },
            {
                "id": 89,
                "catalogGroupId": 5,
                "groupId": 1,
                "access": true
            },
            {
                "id": 87,
                "catalogGroupId": 5,
                "groupId": 2,
                "access": false
            },
            {
                "id": 91,
                "catalogGroupId": 5,
                "groupId": 2,
                "access": true
            }
        ]
    },
    "time": {
        "start": 1794923004,
        "finish": 1794923004.789385,
        "duration": 0.7893850803375244,
        "processing": 0,
        "date_start": "2026-11-17T16:43:24+02:00",
        "date_finish": "2026-11-17T16:43:24+02:00",
        "operating_reset_at": 1794923580,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../../data-types.md) | Array of bindings on the page. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Binding identifier ||
|| **catalogGroupId**
[`integer`](../../../data-types.md) | Price type identifier ||
|| **groupId**
[`integer`](../../../data-types.md) | User group identifier ||
|| **access**
[`boolean`](../../../data-types.md) | Access type:
- `true` — the group can purchase at this price type
- `false` — the group can only view this price type

A group can have two bindings to the same price type: one to view and one to purchase, like groups `1` and `2` in the response example ||
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
                "message": "This request requires data type \"bool\" in field \"access\".",
                "field": "access"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `access` | This request requires data type `bool` in field `access` | Pass `true` or `false` in the filter instead of `Y` or `N` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `PriceTypeGroupDto` | Use only the fields from the `select` parameter list ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["catalogGroupId","=",5]]` ||
|#

#### Filter Operator Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the operators from the [REST 3.0 overview](../../../rest-v3.md#operators) ||
|#

#### Sorting Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `#ORDER#` | Pass the `ASC` or `DESC` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass a positive number in `limit` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check that the user has the "View product catalog" or "Manage price types" access permission ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-group-add-rest-v3.md)
- [{#T}](./catalog-price-type-group-delete-rest-v3.md)
- [{#T}](./catalog-price-type-group-field-list.md)
- [{#T}](./catalog-price-type-group-field-get.md)
- [{#T}](../catalog-price-type-list-rest-v3.md)

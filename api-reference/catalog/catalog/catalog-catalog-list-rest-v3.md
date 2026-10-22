# Get List of Trade Catalogs v 3.0 catalog.catalog.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.catalog.list` method returns a list of trade catalogs with filtering, sorting, and pagination.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of trade catalog fields to return in the response, for example `["id","name","productIblockId"]`.

Available fields:

- `id` — trade catalog identifier
- `iblockId` — catalog information block identifier
- `subscription` — content sales flag
- `vatId` — VAT rate identifier
- `productIblockId` — product information block identifier for an offers catalog
- `skuPropertyId` — identifier of the property that links a variation to its product
- `iblockTypeId` — information block type
- `lid` — site identifier
- `name` — catalog name

The `id` field is returned only if it is specified in `select`. The `["*"]` value is not supported.

Default: all fields. An empty array `[]` also returns all fields ||
|| **filter**
[`array`](../../data-types.md) | List filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`, for example `[["productIblockId", ">", 0]]`. The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter).

You can filter by all fields from the `select` parameter.

The `=`, `!=`, `>`, `>=`, `<`, `<=`, `in` operators are supported. The `between` operator, groups with `or` logic, and negation are not supported.

You cannot pass two conditions with the same operator for one field. An `in` condition with an empty list returns an empty list ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format, for example `{"id": "desc"}`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

You can sort by all fields from the `select` parameter.

Default: `{"id": "asc"}` ||
|| **pagination**
[`object`](../../data-types.md) | Pagination. [Object structure description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of catalogs per page.

Allowed values: from `1` to `1000`. Values greater than `1000` are reduced to `1000`; a zero or negative value causes an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number. The offset is calculated as `(page - 1) * limit`.

If `page` is passed, the `offset` value is ignored ||
|| **offset**
[`integer`](../../data-types.md) | Offset from the beginning of the list. The value must be a multiple of `limit`, for example `0`, `10`, `20` when `limit` is `10`.

Default: `0` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.catalog.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","productIblockId"],"filter":[["iblockTypeId","=","CRM_PRODUCT_CATALOG"]],"order":{"id":"desc"},"pagination":{"limit":10}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","productIblockId"],"filter":[["iblockTypeId","=","CRM_PRODUCT_CATALOG"]],"order":{"id":"desc"},"pagination":{"limit":10},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.catalog.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CatalogListResult = {
      items: Array<{
        id: number
        name: string
        productIblockId: number
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<CatalogListResult>({
        method: 'catalog.catalog.list',
        params: {
          select: [
            'id',
            'name',
            'productIblockId',
          ],
          filter: [
            ['iblockTypeId', '=', 'CRM_PRODUCT_CATALOG'],
          ],
          order: {
            id: 'desc',
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
        console.info('Catalogs:', result.items)
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
      async function listCatalogs() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.catalog.list',
            params: {
              select: [
                'id',
                'name',
                'productIblockId',
              ],
              filter: [
                ['iblockTypeId', '=', 'CRM_PRODUCT_CATALOG'],
              ],
              order: {
                id: 'desc',
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
          console.info('Catalogs:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listCatalogs)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.catalog.list(
            select=["id", "name", "productIblockId"],
            filter=[
                ["iblockTypeId", "=", "CRM_PRODUCT_CATALOG"],
            ],
            order={"id": "desc"},
            pagination={"limit": 10},
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
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
                'catalog.catalog.list',
                [
                    'select' => ['id', 'name', 'productIblockId'],
                    'filter' => [
                        ['iblockTypeId', '=', 'CRM_PRODUCT_CATALOG'],
                    ],
                    'order' => ['id' => 'desc'],
                    'pagination' => ['limit' => 10],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.catalog.list',
        {
            select: ['id', 'name', 'productIblockId'],
            filter: [
                ['iblockTypeId', '=', 'CRM_PRODUCT_CATALOG'],
            ],
            order: { id: 'desc' },
            pagination: { limit: 10 },
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
        'catalog.catalog.list',
        [
            'select' => ['id', 'name', 'productIblockId'],
            'filter' => [
                ['iblockTypeId', '=', 'CRM_PRODUCT_CATALOG'],
            ],
            'order' => ['id' => 'desc'],
            'pagination' => ['limit' => 10],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.catalog.list", b24.Params{
    	"select": []string{"id", "name", "productIblockId"},
    	"filter": []any{
    		[]any{"iblockTypeId", "=", "CRM_PRODUCT_CATALOG"},
    	},
    	"order": b24.Params{
    		"id": "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 10,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.catalog.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID              int    `json:"id"`
    		Name            string `json:"name"`
    		ProductIblockID int    `json:"productIblockId"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Name, item.ProductIblockID)
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
                "id": 27,
                "productIblockId": 25,
                "name": "CRM product catalog (offers)"
            },
            {
                "id": 25,
                "productIblockId": 0,
                "name": "CRM product catalog"
            }
        ]
    },
    "time": {
        "start": 1792665492,
        "finish": 1792665492.231132,
        "duration": 0.23113203048706055,
        "processing": 0,
        "date_start": "2026-10-22T13:38:12+02:00",
        "date_finish": "2026-10-22T13:38:12+02:00",
        "operating_reset_at": 1792666080,
        "operating": 0
    }
}
```

### Returned Data {#returned-data}

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the list of trade catalogs ||
|| **result.items**
[`array`](../../data-types.md) | Array of trade catalogs on the page. The set of fields depends on `select`. If there are no more pages, the array is empty ||
|| **result.items[].id**
[`integer`](../../data-types.md) | Identifier of the trade catalog. Matches `iblockId` ||
|| **result.items[].iblockId**
[`integer`](../../data-types.md) | Identifier of the information block registered as a trade catalog ||
|| **result.items[].subscription**
[`string`](../../data-types.md) | Content sales flag: `Y` — yes, `N` — no. Used only in the on-premise version ||
|| **result.items[].vatId**
[`integer`](../../data-types.md) | Identifier of the catalog VAT rate. `0` — no rate is set ||
|| **result.items[].productIblockId**
[`integer`](../../data-types.md) | Identifier of the product information block to which the offers catalog is linked. `0` — the catalog is not an offers catalog ||
|| **result.items[].skuPropertyId**
[`integer`](../../data-types.md) | Identifier of the property in which a variation stores the identifier of its parent product. `0` — the catalog is not an offers catalog ||
|| **result.items[].iblockTypeId**
[`string`](../../data-types.md) | Information block type. For CRM trade catalogs — `CRM_PRODUCT_CATALOG`, for a catalog based on a universal list — `lists` ||
|| **result.items[].lid**
[`string`](../../data-types.md) | Identifier of the site to which the information block is linked, for example `s1` ||
|| **result.items[].name**
[`string`](../../data-types.md) | Name of the trade catalog information block ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| `-` | Filter operator "between" is not supported | Replace `between` with two conditions using the `>=` and `<=` operators ||
|| `-` | Filter has conflicting conditions for the field "#FIELD#" | Keep one condition per operator for the field. For several values, use `in` ||
|| `-` | pagination.offset must be a non-negative multiple of pagination.limit | Pass an `offset` that is a multiple of `limit`, or use `page` ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["productIblockId",">",0]]` ||
|#

#### Filter Operator Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the `=`, `!=`, `>`, `>=`, `<`, `<=`, `in` operators ||
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
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass a positive number in `limit`, a non-negative number in `offset`, and a number in `page` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `CatalogDto` | Specify only the fields from the `select` parameter description ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | No permission to view trade catalog settings. Call the method on behalf of an administrator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-catalog-get-rest-v3.md)
- [{#T}](./catalog-catalog-is-offers-rest-v3.md)
- [{#T}](./catalog-catalog-field-list.md)
- [{#T}](../../rest-v3.md)

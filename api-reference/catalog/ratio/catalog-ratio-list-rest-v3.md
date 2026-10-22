# Get List of Measurement Unit Ratios v 3.0 catalog.ratio.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "Manage price types" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.ratio.list` method returns a list of measurement unit ratios with filtering, sorting, and pagination.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of ratio fields to return in the response.

Available fields:

- `id` — ratio record identifier
- `productId` — product identifier
- `ratio` — measurement unit ratio
- `isDefault` — whether the ratio is the default one

The `["*"]` value is not supported.

Default: all fields ||
|| **filter**
[`array`](../../data-types.md) | A filter in the REST 3.0 format: an array of conditions in the form `["field", "operator", "value"]`. You can filter by all fields from `select`.

For `isDefault`, pass `true` or `false`; the strings `Y` and `N` cause a validation error. Product identifiers for filtering by `productId` can be retrieved using the [catalog.product.list](../product/catalog-product-list.md) method

The syntax and operators are described in the [REST 3.0 overview](../../rest-v3.md#filter) ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format. You can sort by all fields from `select`.

Possible direction values:

- `ASC` — ascending
- `DESC` — descending

The direction is case-insensitive.

Default: `{"id": "ASC"}` ||
|| **pagination**
[`object`](../../data-types.md) | Pagination object. [Object structure description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of ratios per page. Values greater than `1000` are reduced to `1000`. Zero, a negative value, or a non-numeric value causes an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit`. Zero and a negative value return the first page.

If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../data-types.md) | Number of ratios to skip from the beginning of the list. A negative value causes an error.

Default: `0` ||
|#

{% note info "" %}

The response does not contain the total number of ratios. If a page returns fewer records than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.ratio.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","ratio","isDefault"],"filter":[["productId","=",6461]]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.ratio.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","ratio","isDefault"],"filter":[["productId","=",6461]],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.ratio.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type Ratio = {
      id: number
      productId: number
      ratio: number
      isDefault: boolean
    }

    type RatioListResult = {
      items: Ratio[]
    }

    try {
      const response = await $b24.actions.v3.call.make<RatioListResult>({
        method: 'catalog.ratio.list',
        params: {
          select: ['id', 'productId', 'ratio', 'isDefault'],
          filter: [
            ['productId', '=', 6461],
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Ratios:', result.items)
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
      async function listRatios() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.ratio.list',
            params: {
              select: ['id', 'productId', 'ratio', 'isDefault'],
              filter: [
                ['productId', '=', 6461],
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Ratios:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listRatios)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.ratio.list(
            select=["id", "productId", "ratio", "isDefault"],
            filter=[
                ["productId", "=", 6461],
            ],
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
                'catalog.ratio.list',
                [
                    'select' => ['id', 'productId', 'ratio', 'isDefault'],
                    'filter' => [
                        ['productId', '=', 6461],
                    ],
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
        'catalog.ratio.list',
        {
            select: ['id', 'productId', 'ratio', 'isDefault'],
            filter: [
                ['productId', '=', 6461],
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
        'catalog.ratio.list',
        [
            'select' => ['id', 'productId', 'ratio', 'isDefault'],
            'filter' => [
                ['productId', '=', 6461],
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
    res, err := client.Core().Call(ctx, "catalog.ratio.list", b24.Params{
    	"select": []string{"id", "productId", "ratio", "isDefault"},
    	"filter": []any{
    		[]any{"productId", "=", 6461},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.ratio.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID        int     `json:"id"`
    		ProductID int     `json:"productId"`
    		Ratio     float64 `json:"ratio"`
    		IsDefault bool    `json:"isDefault"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.ProductID, item.Ratio)
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
                "id": 285,
                "productId": 6461,
                "ratio": 10,
                "isDefault": true
            }
        ]
    },
    "time": {
        "start": 1792668107,
        "finish": 1792668107.808,
        "duration": 0.8080000877380371,
        "processing": 0,
        "date_start": "2026-10-22T14:21:47+02:00",
        "date_finish": "2026-10-22T14:21:47+02:00",
        "operating_reset_at": 1792668660,
        "operating": 0
    }
}
```

### Returned Data {#returned-data}

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | Array of ratio records on the page. [Element field description](#items) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

The set of fields depends on `select`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Ratio record identifier ||
|| **productId**
[`integer`](../../data-types.md) | Product identifier ||
|| **ratio**
[`double`](../../data-types.md) | Measurement unit ratio — the step in which the product can be added to the cart ||
|| **isDefault**
[`boolean`](../../data-types.md) | Default ratio: `true` — yes, `false` — no ||
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
                "message": "This request requires data type \"bool\" in field \"isDefault\".",
                "field": "isDefault"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `isDefault` | This request requires data type `bool` in field `isDefault` | Pass `true` or `false` in the filter, not `Y` or `N` ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["productId","=",6461]]` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `#OPERATOR#` | Use the operators from the [REST 3.0 overview](../../rest-v3.md#operators) ||
|#

#### Sorting Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `#ORDER#` | Pass `order` as an object, for example `{"id":"ASC"}`, with the `ASC` or `DESC` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass a positive number in `limit` and a non-negative number in `offset` ||
|#

#### Unknown Field Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `MeasureRatioDto` | Use only the `id`, `productId`, `ratio`, and `isDefault` fields ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["id"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" or "Manage price types" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-ratio-get-rest-v3.md)
- [{#T}](./catalog-ratio-field-list.md)

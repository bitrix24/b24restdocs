# Get List of Units of Measurement v 3.0 catalog.measure.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the catalog or view warehouses

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.measure.list` method returns a list of units of measurement with filtering, sorting, and pagination.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | Array of unit of measurement fields to return in the response.

Available fields:

- `id` — unit of measurement identifier
- `code` — unit of measurement code
- `measureTitle` — name
- `symbol` — symbol
- `symbolIntl` — international symbol
- `symbolLetterIntl` — international letter code symbol
- `isDefault` — default unit flag

Default: all fields. An empty array `[]` also returns all fields; the value `["*"]` is not supported ||
|| **filter**
[`array`](../../data-types.md) | List filter in the REST 3.0 format: an array of conditions like `["field", "operator", "value"]`, for example `[["code","in",[796,166]]]`.

You can filter by all fields from the `select` list. For `isDefault`, pass `true` or `false`.

The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter) ||
|| **order**
[`object`](../../data-types.md) | Sorting in the `{"field": "direction"}` format, for example `{"code":"DESC"}`.

You can sort by all fields from the `select` list.

Possible direction values:

- `ASC` — ascending
- `DESC` — descending ||
|| **pagination**
[`object`](../../data-types.md) | Pagination parameters. [Object structure description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of units of measurement per page. Values greater than `1000` are reduced to `1000`. Zero, negative, and non-numeric values cause an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) * limit`. If `page` is passed, the `offset` value is ignored ||
|| **offset**
[`integer`](../../data-types.md) | Number of records to skip from the beginning of the list.

Default: `0` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.measure.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","code","measureTitle","symbol","isDefault"],"filter":[["code","in",[796,166]]],"order":{"code":"DESC"},"pagination":{"limit":2}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.measure.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","code","measureTitle","symbol","isDefault"],"filter":[["code","in",[796,166]]],"order":{"code":"DESC"},"pagination":{"limit":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.measure.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type MeasureListResult = {
      items: Array<{
        id: number
        code: number
        measureTitle: string
        symbol: string
        isDefault: boolean
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<MeasureListResult>({
        method: 'catalog.measure.list',
        params: {
          select: ['id', 'code', 'measureTitle', 'symbol', 'isDefault'],
          filter: [
            ['code', 'in', [796, 166]],
          ],
          order: {
            code: 'DESC',
          },
          pagination: {
            limit: 2,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Measures:', result.items)
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
      async function listMeasures() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.measure.list',
            params: {
              select: ['id', 'code', 'measureTitle', 'symbol', 'isDefault'],
              filter: [
                ['code', 'in', [796, 166]],
              ],
              order: {
                code: 'DESC',
              },
              pagination: {
                limit: 2,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Measures:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listMeasures)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.measure.list(
            select=["id", "code", "measureTitle", "symbol", "isDefault"],
            filter=[
                ["code", "in", [796, 166]],
            ],
            order={
                "code": "DESC",
            },
            pagination={
                "limit": 2,
            },
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
                'catalog.measure.list',
                [
                    'select' => ['id', 'code', 'measureTitle', 'symbol', 'isDefault'],
                    'filter' => [
                        ['code', 'in', [796, 166]],
                    ],
                    'order' => [
                        'code' => 'DESC',
                    ],
                    'pagination' => [
                        'limit' => 2,
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
        'catalog.measure.list',
        {
            select: ['id', 'code', 'measureTitle', 'symbol', 'isDefault'],
            filter: [
                ['code', 'in', [796, 166]],
            ],
            order: {
                code: 'DESC',
            },
            pagination: {
                limit: 2,
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
        'catalog.measure.list',
        [
            'select' => ['id', 'code', 'measureTitle', 'symbol', 'isDefault'],
            'filter' => [
                ['code', 'in', [796, 166]],
            ],
            'order' => [
                'code' => 'DESC',
            ],
            'pagination' => [
                'limit' => 2,
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
    res, err := client.Core().Call(ctx, "catalog.measure.list", b24.Params{
    	"select": []string{"id", "code", "measureTitle", "symbol", "isDefault"},
    	"filter": []any{
    		[]any{"code", "in", []int{796, 166}},
    	},
    	"order": b24.Params{
    		"code": "DESC",
    	},
    	"pagination": b24.Params{
    		"limit": 2,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.measure.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID           int    `json:"id"`
    		Code         int    `json:"code"`
    		MeasureTitle string `json:"measureTitle"`
    		Symbol       string `json:"symbol"`
    		IsDefault    bool   `json:"isDefault"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Code, item.IsDefault)
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
                "id": 9,
                "code": 796,
                "measureTitle": "",
                "symbol": "",
                "isDefault": true
            },
            {
                "id": 7,
                "code": 166,
                "measureTitle": "",
                "symbol": "",
                "isDefault": false
            }
        ]
    },
    "time": {
        "start": 1792665518,
        "finish": 1792665518.241684,
        "duration": 0.2416839599609375,
        "processing": 0,
        "date_start": "2026-10-22T13:38:38+02:00",
        "date_finish": "2026-10-22T13:38:38+02:00",
        "operating_reset_at": 1792666080,
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
[`array`](../../data-types.md) | Array of units of measurement on the page. The set of fields depends on `select`. [Field description](#items) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

The method does not return the total number of records. If the response contains fewer records than `limit`, the page is the last one.

#### Items Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Unit of measurement identifier ||
|| **code**
[`integer`](../../data-types.md) | Unit of measurement code ||
|| **measureTitle**
[`string`](../../data-types.md) | Name of the unit of measurement. An empty string if the value is not set ||
|| **symbol**
[`string`](../../data-types.md) | Symbol. An empty string if the value is not set ||
|| **symbolIntl**
[`string`](../../data-types.md) | International symbol. An empty string if the value is not set ||
|| **symbolLetterIntl**
[`string`](../../data-types.md) | International letter code symbol. An empty string if the value is not set ||
|| **isDefault**
[`boolean`](../../data-types.md) | `true` — default unit of measurement, `false` — regular unit ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION",
        "message": "Invalid sort order \"UP\"."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `MeasureDto` | Specify only the fields from the [items element description](#items) ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["code","=",796]]` ||
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
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass `limit` as a positive number, `offset` as a non-negative number, and `page` as a number ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have permission to read the catalog or view warehouses ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-measure-add-rest-v3.md)
- [{#T}](./catalog-measure-update-rest-v3.md)
- [{#T}](./catalog-measure-get-rest-v3.md)
- [{#T}](./catalog-measure-delete-rest-v3.md)
- [{#T}](./catalog-measure-field-list.md)

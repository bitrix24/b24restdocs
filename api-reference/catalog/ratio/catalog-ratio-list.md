# Get the list of measurement unit ratios catalog.ratio.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" or "Manage Price Types" access permission

The method `catalog.ratio.list` returns the measurement unit ratios of products matching the filter. If a product has no ratio record, Bitrix24 uses a ratio of 1 for it.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) |
An array with the list of fields to select (see the fields of the [catalog_ratio](../data-types.md#catalog_ratio) object).

If the array is not passed or is empty, the method returns all fields
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected measurement unit ratios in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_ratio](../data-types.md#catalog_ratio) object.

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The `@` and `!@` prefixes work for the `id`, `productId`, and `isDefault` fields. With the `ratio` field, the method returns an error.

Substring search with the `%`, `=%`, `%=`, `!%`, `!=%`, and `!%=` prefixes works only for the `isDefault` field. The method compares numeric field values as a whole: the filter `{"%productId": "64"}` does not find the product with ID `6461`
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected fields of measurement unit ratios in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_ratio](../data-types.md#catalog_ratio) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order

If `order` is not passed, the method returns records in ascending order of `id`
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to control pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

{% note warning "" %}

Write field names in `filter` exactly as they appear in the response: `productId`, `isDefault`. The method skips a condition with a different field name, such as `PRODUCT_ID`, without an error. If there are no other conditions, it returns the ratios of all products.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","ratio","isDefault"],"filter":{"@productId":[533,6461],">ratio":0.5,"isDefault":"Y"},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.ratio.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","ratio","isDefault"],"filter":{"@productId":[533,6461],">ratio":0.5,"isDefault":"Y"},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.ratio.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CatalogRatioItem = {
      id: number,
      productId: number,
      ratio: number,
      isDefault: 'Y' | 'N',
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type RatioListResult = {
      ratios: CatalogRatioItem[],
    }

    try {
      // catalog.ratio.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<RatioListResult>({
        method: 'catalog.ratio.list',
        params: {
          select: ['id', 'productId', 'ratio', 'isDefault'],
          filter: {
            '@productId': [533, 6461],
            '>ratio': 0.5,
            isDefault: 'Y',
          },
          order: {
            id: 'desc',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Ratios:', result.ratios, 'Count:', result.ratios.length)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function fetchRatioList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.ratio.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.ratio.list',
            params: {
              select: ['id', 'productId', 'ratio', 'isDefault'],
              filter: {
                '@productId': [533, 6461],
                '>ratio': 0.5,
                isDefault: 'Y',
              },
              order: {
                id: 'desc',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Ratios:', result.ratios, 'Count:', result.ratios.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchRatioList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.ratio.list(
            select=[
                "id",
                "productId",
                "ratio",
                "isDefault",
            ],
            filter={
                "@productId": [533, 6461],
                ">ratio": 0.5,
                "isDefault": "Y",
            },
            order={
                "id": "desc",
            },
            start=0,
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.ratio.list',
                [
                    'select' => [
                        'id',
                        'productId',
                        'ratio',
                        'isDefault',
                    ],
                    'filter' => [
                        '@productId' => [533, 6461],
                        '>ratio'     => 0.5,
                        'isDefault'  => 'Y',
                    ],
                    'order' => [
                        'id' => 'desc',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching ratio list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.ratio.list',
            {
                select:[
                    'id',
                    'productId',
                    'ratio',
                    'isDefault',
                ],
                filter:{
                    '@productId': [533, 6461],
                    '>ratio': 0.5,
                    'isDefault': 'Y',
                },
                order:{
                    id: 'desc',
                },
            },
            function(result)
            {
                if(result.error()) {
                    console.error(result.error());
                } else {
                    console.log(result.data());
                }
            }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.ratio.list',
        [
            'select' => [
                'id',
                'productId',
                'ratio',
                'isDefault',
            ],
            'filter' => [
                '@productId' => [533, 6461],
                '>ratio' => 0.5,
                'isDefault' => 'Y',
            ],
            'order' => [
                'id' => 'desc',
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
    	"filter": b24.Params{
    		"@productId": []int{533, 6461},
    		">ratio":     0.5,
    		"isDefault":  "Y",
    	},
    	"order": b24.Params{
    		"id": "desc",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.ratio.list: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ratios": [
            {
                "id": 285,
                "isDefault": "Y",
                "productId": 6461,
                "ratio": 10
            },
            {
                "id": 279,
                "isDefault": "Y",
                "productId": 533,
                "ratio": 1
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1790852349,
        "finish": 1790852349.935266,
        "duration": 0.9352660179138184,
        "processing": 0,
        "date_start": "2026-10-01T13:59:09+03:00",
        "date_finish": "2026-10-01T13:59:09+03:00",
        "operating_reset_at": 1790852949,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **ratios**
[`catalog_ratio[]`](../data-types.md#catalog_ratio) | An array of objects with information about the selected measurement unit ratios ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **next**
[`integer`](../../data-types.md) | The `start` parameter value for retrieving the next page. The field is absent if the last page is retrieved ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "200040300010",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has neither the "View Product Catalog" nor the "Manage Price Types" access permission ||
|| `400` | `100` | Invalid order "<VALUE>" | The sort direction passed in `order` is other than `asc` and `desc` ||
|| `400` | `100` | Order must be a string | The sort direction in `order` is not passed as a string ||
|| `400` | `100` | Invalid value {<VALUE>} to match with parameter {filter}. Should be value of type array. | `filter` is not passed as an object. The same text with `{order}` or `{select}` means that `order` is not passed as an object or `select` is not passed as an array ||
|| `400` | `0` | Call to a member function compile() on float | An array with the `@` or `!@` prefix is passed in `filter` for the `ratio` field ||
|| — | `0` | — | Other errors, such as fatal errors ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./catalog-ratio-get.md)
- [{#T}](./catalog-ratio-get-fields.md)
- [{#T}](../product/catalog-product-list.md)


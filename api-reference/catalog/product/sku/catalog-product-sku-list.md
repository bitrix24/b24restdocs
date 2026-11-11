# Get List of Parent Products catalog.product.sku.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission and the permission to read the catalog information block

The `catalog.product.sku.list` method returns a list of catalog parent products by filter. The list includes parent products with variations (type `3`) and without variations (type `6`).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select***
[`array`](../../../data-types.md) | An array of fields to return. The fields are described in the [catalog_product_sku](../../data-types.md#catalog_product_sku) object.

Make sure to pass the `id` and `iblockId` fields. Property values are selected with `propertyN` keys, where `N` is the property identifier.

In `select`, you can list trade catalog fields such as `canBuyZero`, `quantity`, and `vatId`: the `get`, `add`, and `update` methods do not return them.

The method skips unknown fields and uppercase names, such as `NAME`, without an error ||
|| **filter***
[`object`](../../../data-types.md) | An object for filtering parent products in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Make sure to pass `iblockId`, the identifier of the product catalog information block. You can retrieve it using the [catalog.catalog.list](../../catalog/catalog-catalog-list.md) method.

You can filter by the fields of the [catalog_product_sku](../../data-types.md#catalog_product_sku) object and by single properties, for example `{"property303": "Textile Plus"}`. A filter by a multiple property is not applied.

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `=` — equal, exact match (used by default). An array of values works as IN: `{"id": [7679, 7637]}`
- `!=` — not equal
- `!` — not equal. With an array of values, it works as NOT IN
- `>` — greater than
- `>=` — greater than or equal to
- `<` — less than
- `<=` — less than or equal to
- `%` — LIKE, substring search in any position. The `%` character does not need to be passed in the value
- `!%` — NOT LIKE, values without the substring

The method does not apply the `=%`, `%=`, `!=%`, `!%=`, `@`, and `!@` prefixes and returns records without taking such a condition into account.

The `type` field filters only by the `3` and `6` values. Another value does not narrow the selection.

The method skips unknown fields and uppercase names, such as `ID`, without an error and returns products without taking such a condition into account ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting parent products in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order

The direction is case-insensitive. The method skips unknown fields and an invalid direction without an error.

By default, results are sorted in ascending order by `id` ||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

{% note warning "" %}

A `start` value beyond the list returns the first page, not an empty array. Do not stop the iteration on an empty page: the loop will not end. Rely on the `total` field and on whether the `next` field is present in the response.

{% endnote %}

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

The example retrieves parent products of section `31` with a sort index of at least `100` and a symbolic code that contains `t-shirt`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","sort","code","iblockSectionId","property303"],"filter":{"iblockId":25,"iblockSectionId":31,">=sort":100,"%code":"t-shirt"},"order":{"sort":"asc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.sku.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","sort","code","iblockSectionId","property303"],"filter":{"iblockId":25,"iblockSectionId":31,">=sort":100,"%code":"t-shirt"},"order":{"sort":"asc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.sku.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of one item returned in result.units (only the selected fields)
    type SkuListItem = {
      id: number
      iblockId: number
      name: string
      active: string
      sort: number | null
      code: string | null
      iblockSectionId: number | null
      property303: { value: string; valueId: string } | null
    }

    try {
      // catalog.product.sku.list returns a single page (max 50 records).
      // The list is returned under the "units" key.
      const response = await $b24.actions.v2.call.make<{ units: SkuListItem[] }>({
        method: 'catalog.product.sku.list',
        params: {
          select: ['id', 'iblockId', 'name', 'active', 'sort', 'code', 'iblockSectionId', 'property303'],
          filter: {
            iblockId: 25,
            iblockSectionId: 31,
            '>=sort': 100,
            '%code': 't-shirt',
          },
          order: {
            sort: 'asc',
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
        console.info('SKU products on page:', result.units.length)
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
      async function listSkuProducts() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.sku.list',
            params: {
              select: ['id', 'iblockId', 'name', 'active', 'sort', 'code', 'iblockSectionId', 'property303'],
              filter: {
                iblockId: 25,
                iblockSectionId: 31,
                '>=sort': 100,
                '%code': 't-shirt',
              },
              order: {
                sort: 'asc',
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
          console.info('SKU products on page:', result.units.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listSkuProducts)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.sku.list(
            select=["id", "iblockId", "name", "active", "sort", "code", "iblockSectionId", "property303"],
            filter={
                "iblockId": 25,
                "iblockSectionId": 31,
                ">=sort": 100,
                "%code": "t-shirt",
            },
            order={
                "sort": "asc",
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
                'catalog.product.sku.list',
                [
                    'select' => ['id', 'iblockId', 'name', 'active', 'sort', 'code', 'iblockSectionId', 'property303'],
                    'filter' => [
                        'iblockId'        => 25,
                        'iblockSectionId' => 31,
                        '>=sort'          => 100,
                        '%code'           => 't-shirt',
                    ],
                    'order'  => [
                        'sort' => 'asc',
                    ],
                    'start'  => 0,
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

    ```js
    BX24.callMethod(
        'catalog.product.sku.list',
        {
            select: ['id', 'iblockId', 'name', 'active', 'sort', 'code', 'iblockSectionId', 'property303'],
            filter: {
                iblockId: 25,
                iblockSectionId: 31,
                '>=sort': 100,
                '%code': 't-shirt',
            },
            order: {
                sort: 'asc',
            },
            start: 0,
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.sku.list',
        [
            'select' => ['id', 'iblockId', 'name', 'active', 'sort', 'code', 'iblockSectionId', 'property303'],
            'filter' => [
                'iblockId' => 25,
                'iblockSectionId' => 31,
                '>=sort' => 100,
                '%code' => 't-shirt',
            ],
            'order' => [
                'sort' => 'asc',
            ],
            'start' => 0,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.sku.list", b24.Params{
    	"select": []string{"id", "iblockId", "name", "active", "sort", "code", "iblockSectionId", "property303"},
    	"filter": b24.Params{
    		"iblockId":        25,
    		"iblockSectionId": 31,
    		">=sort":          100,
    		"%code":           "t-shirt",
    	},
    	"order": b24.Params{
    		"sort": "asc",
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.sku.list: %w", err)
    }

    // The method returns the list under the "units" key.
    raw, ok := b24.Unwrap(res.Result, "units")
    if !ok {
    	return fmt.Errorf("no units key in the response")
    }

    // Optional fields can be null, so pointers are used for them.
    var items []struct {
    	ID              b24.ID  `json:"id"`
    	IblockID        int     `json:"iblockId"`
    	Name            string  `json:"name"`
    	Active          string  `json:"active"`
    	Sort            *int    `json:"sort"`
    	Code            *string `json:"code"`
    	IblockSectionID *int    `json:"iblockSectionId"`
    	Property303     *struct {
    		Value   string `json:"value"`
    		ValueID string `json:"valueId"`
    	} `json:"property303"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Name)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "units": [
            {
                "active": "Y",
                "code": "basic-t-shirt",
                "iblockId": 25,
                "iblockSectionId": 31,
                "id": 7679,
                "name": "Basic T-shirt",
                "property303": {
                    "value": "Cotton Yard",
                    "valueId": "4117"
                },
                "sort": 300
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1793893107,
        "finish": 1793893107.413658,
        "duration": 0.41365790367126465,
        "processing": 0,
        "date_start": "2026-11-05T18:38:27+01:00",
        "date_finish": "2026-11-05T18:38:27+01:00",
        "operating_reset_at": 1793893200,
        "operating": 0.13479113578796387
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **result.units**
[`catalog_product_sku[]`](../../data-types.md#catalog_product_sku) | Array of parent products. The method returns the list under the `units` key, not `skus`.

Each product contains only the fields from `select`. The value of a filled property is a `{"value": ..., "valueId": ...}` object, of a multiple property — an array of such objects ||
|| **total**
[`integer`](../../../data-types.md) | Total number of parent products found ||
|| **next**
[`integer`](../../../data-types.md) | The `start` value for the next page. Returned if more than 50 records are found and the page is not the last one ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required filter fields: iblockId"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user does not have a permission to view the catalog ||
|| `400` | `200040300030` | Access Denied | The user does not have the permission to read the catalog information block ||
|| `400` | `0` | Required filter fields: iblockId | `iblockId` is not provided in `filter` ||
|| `400` | `0` | Required select fields: iblockId | `select` does not contain the `iblockId` field. If the `id` field is missing, the `Required select fields: id` error is returned ||
|| `400` | `0` | iblock is not catalog | The `iblockId` information block is not a trade catalog ||
|| `400` | `0` | productType is not allowed for this catalog | `iblockId` contains the variation information block ||
|| `400` | `100` | Invalid value {abc} to match with parameter {filter}. Should be value of type array. | `filter` is not an object. The same error with `{select}` is returned if `select` is not an array ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-sku-add.md)
- [{#T}](./catalog-product-sku-update.md)
- [{#T}](./catalog-product-sku-get.md)
- [{#T}](./catalog-product-sku-download.md)
- [{#T}](./catalog-product-sku-delete.md)
- [{#T}](./catalog-product-sku-get-fields-by-filter.md)

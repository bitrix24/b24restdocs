# Get a list of trade catalogs catalog.catalog.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `catalog.catalog.list` returns a list of trade catalogs.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | Array of fields to return. The fields of the [catalog_catalog](../data-types.md#catalog_catalog) object are available: `id`, `iblockId`, `iblockTypeId`, `lid`, `name`, `productIblockId`, `skuPropertyId`, `subscription`, `vatId`.

If the parameter is not passed or an empty array is passed, the method returns all fields. The method skips unknown fields without an error ||
|| **filter**
[`object`](../../data-types.md) | Object for filtering trade catalogs in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Pass the fields of the [catalog_catalog](../data-types.md#catalog_catalog) object in `field`. The method skips a condition with an unknown field without an error.

You can add a prefix to the key that sets the condition:
- `=` — equals. Used by default
- `!` or `!=` — not equal
- `>` — greater than
- `>=` — greater than or equal to
- `<` — less than
- `<=` — less than or equal to
- `@` — in the list, the value is passed as an array
- `!@` — not in the list, the value is passed as an array
- `%` — contains a substring. Do not pass the `%` character in the value

The method does not support the `=%`, `%=`, `!%`, `!=%`, and `!%=` prefixes: conditions with them are not applied ||
|| **order**
[`object`](../../data-types.md) | Object for sorting trade catalogs in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Pass the fields of the [catalog_catalog](../data-types.md#catalog_catalog) object in `field`.

Possible values for `order`:
- `asc` — ascending
- `desc` — descending

Default: `{"id": "asc"}` ||
|| **start**
[`integer`](../../data-types.md) | Parameter for pagination.

The page size is 50 records. To get the second page, pass `50`, the third page — `100`, and so on.

Formula for calculating the value: `start = (N-1) * 50`, where `N` is the number of the required page ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","iblockTypeId","lid","name","productIblockId","skuPropertyId","subscription","vatId"],"filter":{">=id":25,"@vatId":[1,2],"productIblockId":25},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","iblockTypeId","lid","name","productIblockId","skuPropertyId","subscription","vatId"],"filter":{">=id":25,"@vatId":[1,2],"productIblockId":25},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.catalog.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CatalogCatalogListResult = {
      catalogs: {
        id: number
        iblockId: number
        iblockTypeId: string
        lid: string
        name: string
        productIblockId: number | null
        skuPropertyId: number | null
        subscription: 'Y' | 'N'
        vatId: number | null
      }[]
    }

    try {
      // catalog.catalog.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<CatalogCatalogListResult>({
        method: 'catalog.catalog.list',
        params: {
          select: [
            'id',
            'iblockId',
            'iblockTypeId',
            'lid',
            'name',
            'productIblockId',
            'skuPropertyId',
            'subscription',
            'vatId',
          ],
          filter: {
            '>=id': 25,
            '@vatId': [1, 2],
            productIblockId: 25,
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
        console.info('Catalogs count:', result.catalogs.length, 'First:', result.catalogs[0])
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
      async function listCatalogs() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.catalog.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.catalog.list',
            params: {
              select: [
                'id',
                'iblockId',
                'iblockTypeId',
                'lid',
                'name',
                'productIblockId',
                'skuPropertyId',
                'subscription',
                'vatId',
              ],
              filter: {
                '>=id': 25,
                '@vatId': [1, 2],
                productIblockId: 25,
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
          console.info('Catalogs count:', result.catalogs.length, 'First:', result.catalogs[0])
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
            select=[
                "id",
                "iblockId",
                "iblockTypeId",
                "lid",
                "name",
                "productIblockId",
                "skuPropertyId",
                "subscription",
                "vatId",
            ],
            filter={
                ">=id": 25,
                "@vatId": [
                    1,
                    2,
                ],
                "productIblockId": 25,
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
                'catalog.catalog.list',
                [
                    'select' => [
                        'id',
                        'iblockId',
                        'iblockTypeId',
                        'lid',
                        'name',
                        'productIblockId',
                        'skuPropertyId',
                        'subscription',
                        'vatId',
                    ],
                    'filter' => [
                        '>=id' => 25,
                        '@vatId' => [1, 2],
                        'productIblockId' => 25,
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
        echo 'Error fetching catalog list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "catalog.catalog.list", {
            "select": [
                "id",
                "iblockId",
                "iblockTypeId",
                "lid",
                "name",
                "productIblockId",
                "skuPropertyId",
                "subscription",
                "vatId",
            ],
            "filter": {
                ">=id": 25,
                "@vatId": [1, 2],
                "productIblockId": 25
            },
            "order": {
                "id": "desc"
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.catalog.list',
        [
            'select' => [
                'id',
                'iblockId',
                'iblockTypeId',
                'lid',
                'name',
                'productIblockId',
                'skuPropertyId',
                'subscription',
                'vatId',
            ],
            'filter' => [
                '>=id' => 25,
                '@vatId' => [1, 2],
                'productIblockId' => 25,
            ],
            'order' => [
                'id' => 'desc',
            ]
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
    	"select": []string{"id", "iblockId", "iblockTypeId", "lid", "name", "productIblockId", "skuPropertyId", "subscription", "vatId"},
    	"filter": b24.Params{
    		">=id":            25,
    		"@vatId":          []int{1, 2},
    		"productIblockId": 25,
    	},
    	"order": b24.Params{
    		"id": "desc",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.catalog.list: %w", err)
    }

    // The method wraps the response in an object with the "catalogs" key.
    raw, ok := b24.Unwrap(res.Result, "catalogs")
    if !ok {
    	return fmt.Errorf("no catalogs key in the response")
    }

    var items []struct {
    	IblockID        b24.ID `json:"iblockId"`
    	IblockTypeID    string `json:"iblockTypeId"`
    	ID              b24.ID `json:"id"`
    	Lid             string `json:"lid"`
    	Name            string `json:"name"`
    	ProductIblockID b24.ID `json:"productIblockId"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.IblockID)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "catalogs": [
            {
                "iblockId": 27,
                "iblockTypeId": "CRM_PRODUCT_CATALOG",
                "id": 27,
                "lid": "s1",
                "name": "Product Catalog CRM (offers)",
                "productIblockId": 25,
                "skuPropertyId": 101,
                "subscription": "N",
                "vatId": 1
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1793613124,
        "finish": 1793613124.823565,
        "duration": 0.8235650062561035,
        "processing": 0,
        "date_start": "2026-11-02T12:52:04+03:00",
        "date_finish": "2026-11-02T12:52:04+03:00",
        "operating_reset_at": 1793613720,
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
|| **result.catalogs**
[`catalog_catalog[]`](../data-types.md#catalog_catalog) | Array of objects with information about the selected trade catalogs ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
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
|| `400` | `200040300010` | Access Denied | The user does not have permission to view Bitrix24 settings ||
|| `400` | `200040300030` | Access Denied | The user does not have permission to read the trade catalog ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-catalog-get.md)
- [{#T}](./catalog-catalog-is-offers.md)
- [{#T}](./catalog-catalog-get-fields.md)
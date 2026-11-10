# Get List of Product Variations catalog.product.offer.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the variations information block and one of the permissions: "View product catalog", "View catalog products", or "Edit selling price"

The method `catalog.product.offer.list` returns a list of product variations with filtering, sorting, and pagination.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select***
[`array`](../../../data-types.md) | Array of variation fields to return. The fields are described in the [catalog_product_offer](../../data-types.md#catalog_product_offer) object. Pass properties as `propertyN`, where `N` is the property identifier, and the parent product as `parentId`.

The array must contain the `id` and `iblockId` fields, otherwise the method returns the `Required select fields` error. Write field names in camelCase. The method skips unknown fields without an error ||
|| **filter***
[`object`](../../../data-types.md) | Object for filtering variations in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Be sure to pass `iblockId`, the identifier of the variations catalog information block. You can retrieve it using the [catalog.catalog.list](../../catalog/catalog-catalog-list.md) method: the variations catalog has the `productIblockId` field filled in.

Possible values for `field` are the fields of the [catalog_product_offer](../../data-types.md#catalog_product_offer) object, `propertyN` properties, and `parentId`, the parent product identifier. A key can be assigned a prefix:
- `=` — equal to, used by default. An array of values works as IN: `{"id": [7631, 7671]}`
- `!=` — not equal to
- `!` — not equal to. An array of values works as NOT IN
- `>` — greater than
- `>=` — greater than or equal to
- `<` — less than
- `<=` — less than or equal to
- `%` — LIKE, substring search in any position. The `%` character does not need to be passed in the value
- `!%` — NOT LIKE, values without the substring. The `%` character does not need to be passed in the value

The method does not support the `@`, `!@`, `=%`, `%=`, `!=%`, and `!%=` prefixes: a condition with such a prefix is not applied, and the method returns variations without taking it into account. For IN and NOT IN, pass an array without a prefix or with the `!` prefix.

To find an empty value, pass `false`, for example `{"code": false}`. The `null` value causes the `Wrong format value of filter field` error.

For a list property, pass the identifier of the value option, for example `{"property907": 1605}`. Filter a date property by range: specify a prefix in the key and pass the value in the `value` field:

```json
{
    "iblockId": 27,
    ">=property925": {"value": "2026-11-05T00:00:00+01:00"},
    "<=property925": {"value": "2026-11-06T00:00:00+01:00"}
}
```

The method skips unknown fields and uppercase names, for example `ID`, without an error and returns variations without taking such a condition into account. Filter by the parent product using `parentId`: a condition on the link property, for example `property101`, is not applied
||
|| **order**
[`object`](../../../data-types.md) | Object for sorting variations in the format `{"field_1": "order_1", ..., "field_N": "order_N"}`.

Possible values for `order`:
- `asc` — ascending
- `desc` — descending

The `desc` direction and any value other than `asc` sort in descending order. The direction is case-insensitive. The method skips unknown fields without an error.

By default, variations are sorted by `id` in ascending order ||
|| **start**
[`integer`](../../../data-types.md) | The parameter is used for pagination.

The results page size is always static — 50 records.

To select the second page of results, pass `50`. To select the third page, pass `100`, and so on.

Formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the number of the required page.

If `start` is greater than the number of variations found, the method returns the first page rather than an empty list. To go through all pages, rely on the `next` field in the response: it is absent on the last page ||
|#

The method does not return variation prices. To retrieve prices, use the [catalog.price.*](../../price/index.md) methods.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

The example retrieves active variations of the parent product `7603` with sizes `M` and `L` in the `property907` list property and sorts them by the `sort` field.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","sort","parentId","property907"],"filter":{"iblockId":27,"parentId":7603,"active":"Y","property907":[1603,1605]},"order":{"sort":"asc","id":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","sort","parentId","property907"],"filter":{"iblockId":27,"parentId":7603,"active":"Y","property907":[1603,1605]},"order":{"sort":"asc","id":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type OfferProperty = {
      value: string
      valueId: string
      valueEnum?: string
    }

    // Shape of one offer returned in result.offers (only the selected fields)
    type OfferListItem = {
      id: number
      iblockId: number
      name: string
      active: string
      sort: number | null
      parentId: OfferProperty | null
      property907: OfferProperty | null
    }

    try {
      // catalog.product.offer.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<{ offers: OfferListItem[] }>({
        method: 'catalog.product.offer.list',
        params: {
          select: ['id', 'iblockId', 'name', 'active', 'sort', 'parentId', 'property907'],
          filter: {
            iblockId: 27,
            parentId: 7603,
            active: 'Y',
            property907: [1603, 1605],
          },
          order: {
            sort: 'asc',
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
        console.info('Offers on page:', result.offers.length)
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
      async function listOffers() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.offer.list',
            params: {
                  select: ['id', 'iblockId', 'name', 'active', 'sort', 'parentId', 'property907'],
                  filter: {
                    iblockId: 27,
                    parentId: 7603,
                    active: 'Y',
                    property907: [1603, 1605],
                  },
                  order: {
                    sort: 'asc',
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
          console.info('Offers on page:', result.offers.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listOffers)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.offer.list(
            select=["id", "iblockId", "name", "active", "sort", "parentId", "property907"],
            filter={
                "iblockId": 27,
                "parentId": 7603,
                "active": "Y",
                "property907": [1603, 1605],
            },
            order={
                "sort": "asc",
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
                'catalog.product.offer.list',
                [
                    'select' => ['id', 'iblockId', 'name', 'active', 'sort', 'parentId', 'property907'],
                    'filter' => [
                        'iblockId'    => 27,
                        'parentId'    => 7603,
                        'active'      => 'Y',
                        'property907' => [1603, 1605],
                    ],
                    'order'  => [
                        'sort' => 'asc',
                        'id'   => 'desc',
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
        'catalog.product.offer.list',
        {
            select: ['id', 'iblockId', 'name', 'active', 'sort', 'parentId', 'property907'],
            filter: {
                iblockId: 27,
                parentId: 7603,
                active: 'Y',
                property907: [1603, 1605],
            },
            order: {
                sort: 'asc',
                id: 'desc',
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
        'catalog.product.offer.list',
        [
            'select' => ['id', 'iblockId', 'name', 'active', 'sort', 'parentId', 'property907'],
            'filter' => [
                'iblockId' => 27,
                'parentId' => 7603,
                'active' => 'Y',
                'property907' => [1603, 1605],
            ],
            'order' => [
                'sort' => 'asc',
                'id' => 'desc',
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
    res, err := client.Core().Call(ctx, "catalog.product.offer.list", b24.Params{
    	"select": []string{"id", "iblockId", "name", "active", "sort", "parentId", "property907"},
    	"filter": b24.Params{
    		"iblockId":    27,
    		"parentId":    7603,
    		"active":      "Y",
    		"property907": []int{1603, 1605},
    	},
    	"order":  b24.Params{
    		"sort": "asc",
    		"id":   "desc",
    	},
    	"start":  0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.offer.list: %w", err)
    }

    // The method wraps the response in an object with the "offers" key.
    raw, ok := b24.Unwrap(res.Result, "offers")
    if !ok {
    	return fmt.Errorf("the response has no offers key")
    }

    // Optional fields can be null, so pointers are used for them.
    var items []struct {
    	ID       b24.ID `json:"id"`
    	IblockID int    `json:"iblockId"`
    	Name     string `json:"name"`
    	Sort     *int   `json:"sort"`
    	ParentID *struct {
    		Value string `json:"value"`
    	} `json:"parentId"`
    	Property907 *struct {
    		Value     string `json:"value"`
    		ValueEnum string `json:"valueEnum"`
    	} `json:"property907"`
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
        "offers": [
            {
                "active": "Y",
                "iblockId": 27,
                "id": 7695,
                "name": "Red T-shirt, M",
                "parentId": {
                    "value": "7603",
                    "valueId": "4149"
                },
                "property907": {
                    "value": "1603",
                    "valueEnum": "M",
                    "valueId": "4157"
                },
                "sort": 100
            },
            {
                "active": "Y",
                "iblockId": 27,
                "id": 7685,
                "name": "Blue T-shirt, M",
                "parentId": {
                    "value": "7603",
                    "valueId": "4125"
                },
                "property907": {
                    "value": "1603",
                    "valueEnum": "M",
                    "valueId": "4143"
                },
                "sort": 100
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1793895575,
        "finish": 1793895575.244894,
        "duration": 0.24489402770996094,
        "processing": 0,
        "date_start": "2026-11-05T19:19:35+01:00",
        "date_finish": "2026-11-05T19:19:35+01:00",
        "operating_reset_at": 1793896140,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **result.offers**
[`catalog_product_offer[]`](../../data-types.md#catalog_product_offer) | Array of variations. Each variation contains only the fields from `select`, and empty fields are returned as `null`.

The parent product and properties are returned as an object `{"value": ..., "valueId": ...}`, for a list property with the option text in `valueEnum`. The `purchasingPrice` and `purchasingCurrency` fields are returned only if the user has the "View purchase price" permission ||
|| **next**
[`integer`](../../../data-types.md) | The `start` value for the next page. Returned if more records are found than fit on the current page ||
|| **total**
[`integer`](../../../data-types.md) | Total number of variations found ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required select fields: iblockId"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" permissions ||
|| `400` | `200040300030` | Access Denied | The user does not have permission to read the variations information block ||
|| `400` | `0` | Required select fields: id, iblockId | `select` does not contain the `id` and `iblockId` fields, is empty, or was not provided. The text lists the missing fields ||
|| `400` | `0` | Required filter fields: iblockId | `iblockId` was not provided in `filter` ||
|| `400` | `0` | Wrong format value of filter field `code`. | `null` was passed as a filter value. The text contains the filter key ||
|| `400` | `0` | productType is not allowed for this catalog | The product information block was passed in `iblockId` instead of the variations information block ||
|| `400` | `0` | iblock is not catalog | The information block with the specified `iblockId` was not found or is not a catalog ||
|| `400` | `100` | Invalid value {abc} to match with parameter {filter}. Should be value of type array. | `filter` is not an object. The same error with `{select}` occurs if `select` is not an array ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-add.md)
- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-get.md)
- [{#T}](./catalog-product-offer-download.md)
- [{#T}](./catalog-product-offer-delete.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)

# Get a List of Product Property Feature Parameters or Variations catalog.productPropertyFeature.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" access permission

The method `catalog.productPropertyFeature.list` returns a list of product property feature parameters and variations based on the filter.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see the fields of the object [catalog_product_property_features](../data-types.md#catalog_product_property_features)).

If the parameter is not provided, all fields will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected parameters in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_property_features](../data-types.md#catalog_product_property_features).

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value
- `=%` — LIKE, substring search. The `%` symbol should be included in the value
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal

If `propertyId` is not provided, the method selects parameters for all properties of the trade catalogs. If a `propertyId` is provided that does not exist or does not relate to the trade catalog, the method will return an empty list. ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected parameters in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_property_features](../data-types.md#catalog_product_property_features).

Possible values for `order`:
- `ASC` — in ascending order
- `DESC` — in descending order

If the parameter is not provided, sorting `id ASC` is applied. ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — `100`, and so on.

The formula for calculating the `start` parameter value: `start = (N - 1) * 50`, where `N` is the desired page number.

If the value `-1` is passed, the response will not include the `total` field. ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"select":["id","propertyId","moduleId","featureId","isEnabled"],"filter":{"propertyId":901},"order":{"id":"ASC"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyFeature.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"select":["id","propertyId","moduleId","featureId","isEnabled"],"filter":{"propertyId":901},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyFeature.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductPropertyFeatureListResult = {
      productPropertyFeatures: {
        id: number
        propertyId: number
        moduleId: string
        featureId: string
        isEnabled: string
      }[]
    }

    // catalog.productPropertyFeature.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<ProductPropertyFeatureListResult>({
        method: 'catalog.productPropertyFeature.list',
        params: {
          select: ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
          filter: { propertyId: 901 },
          order: { id: 'ASC' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.productPropertyFeatures.length, result.productPropertyFeatures)
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
      async function listProductPropertyFeatures() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.productPropertyFeature.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.productPropertyFeature.list',
            params: {
              select: ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
              filter: { propertyId: 901 },
              order: { id: 'ASC' },
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
          console.info(result.productPropertyFeatures.length, result.productPropertyFeatures)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listProductPropertyFeatures)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productPropertyFeature.list',
                [
                    'select' => ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
                    'filter' => [
                        'propertyId' => 901,
                    ],
                    'order' => ['id' => 'ASC'],
                ]
            );

        print_r($response->getResponseData()->getResult());
    } catch (\Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productPropertyFeature.list',
        {
            select: ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
            filter: {
                propertyId: 901
            },
            order: { id: 'ASC' }
        },
        function(result) {
            if (result.error()) {
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
        'catalog.productPropertyFeature.list',
        [
            'select' => ['id', 'propertyId', 'moduleId', 'featureId', 'isEnabled'],
            'filter' => ['propertyId' => 901],
            'order' => ['id' => 'ASC'],
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "productPropertyFeatures": [
        {
            "featureId": "DETAIL_PAGE_SHOW",
            "id": 99,
            "isEnabled": "Y",
            "moduleId": "iblock",
            "propertyId": 901
        },
        {
            "featureId": "LIST_PAGE_SHOW",
            "id": 101,
            "isEnabled": "Y",
            "moduleId": "iblock",
            "propertyId": 901
        }
        ]
    },
    "total": 2,
    "time": {
        "start": 1774254804,
        "finish": 1774254805.034701,
        "duration": 1.0347011089324951,
        "processing": 1,
        "date_start": "2026-03-23T11:33:24+01:00",
        "date_finish": "2026-03-23T11:33:25+01:00",
        "operating_reset_at": 1774255404,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root object of the response ||
|| **productPropertyFeatures**
[`catalog_product_property_features[]`](../data-types.md#catalog_product_property_features) | An array of objects containing information about the selected parameters ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. This field is returned if there are more records ||
|| **total**
[`integer`](../../data-types.md) | The total number of records. This field is not returned if the request is executed with `start = -1` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Access Denied"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access Denied | Insufficient rights to view the catalog ||
|| `100` | Invalid value {wrong_type} to match with parameter {filter}. Should be value of type array. | Invalid data type for the `filter` parameter value ||
|| `100` | Invalid value {wrong_type} to match with parameter {select}. Should be value of type array. | Invalid data type for the `select` parameter value ||
|| `100` | Invalid value {wrong_type} to match with parameter {order}. Should be value of type array. | Invalid data type for the `order` parameter value ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-feature-add.md)
- [{#T}](./catalog-product-property-feature-update.md)
- [{#T}](./catalog-product-property-feature-get.md)
- [{#T}](./catalog-product-property-feature-get-available-features-by-property.md)
- [{#T}](./catalog-product-property-feature-get-fields.md)
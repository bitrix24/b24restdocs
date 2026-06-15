# Get a List of Values for List Properties catalog.productPropertyEnum.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" access permission

The method `catalog.productPropertyEnum.list` returns a list of values for list properties based on the filter.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to be selected (see fields of the object [catalog_product_property_enum](../data-types.md#catalog_product_property_enum)).

If the parameter is not provided, all fields will be selected ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected values in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_property_enum](../data-types.md#catalog_product_property_enum).

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
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
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
  
If `propertyId` is not provided, the method selects values for all list properties in the product catalogs. If a `propertyId` is provided that does not exist or does not belong to the product catalog, the method will return an empty list ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected values in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_property_enum](../data-types.md#catalog_product_property_enum).

Possible values for `order`:
- `ASC` — in ascending order
- `DESC` — in descending order

If the parameter is not provided, sorting `id ASC` is applied ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — `100`, and so on.

The formula for calculating the `start` parameter value: `start = (N - 1) * 50`, where `N` is the desired page number.

If the value `-1` is passed, the response will not include the `total` field ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"select":["id","propertyId","value","def","sort","xmlId"],"filter":{"propertyId":431},"order":{"id":"ASC"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyEnum.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"select":["id","propertyId","value","def","sort","xmlId"],"filter":{"propertyId":431},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyEnum.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductPropertyEnumItem = {
      def: string
      id: number
      propertyId: number
      sort: number
      value: string
      xmlId: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductPropertyEnumListResult = {
      productPropertyEnums: ProductPropertyEnumItem[]
    }

    try {
      // catalog.productPropertyEnum.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ProductPropertyEnumListResult>({
        method: 'catalog.productPropertyEnum.list',
        params: {
          select: ['id', 'propertyId', 'value', 'def', 'sort', 'xmlId'],
          filter: { propertyId: 431 },
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
        console.info('Property enum values:', result.productPropertyEnums, 'count:', result.productPropertyEnums.length)
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
      async function fetchProductPropertyEnumList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.productPropertyEnum.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.productPropertyEnum.list',
            params: {
              select: ['id', 'propertyId', 'value', 'def', 'sort', 'xmlId'],
              filter: { propertyId: 431 },
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
          console.info('Property enum values:', result.productPropertyEnums, 'count:', result.productPropertyEnums.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchProductPropertyEnumList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productPropertyEnum.list',
                [
                    'select' => ['id', 'propertyId', 'value', 'def', 'sort', 'xmlId'],
                    'filter' => [
                        'propertyId' => 431,
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
        'catalog.productPropertyEnum.list',
        {
            select: ['id', 'propertyId', 'value', 'def', 'sort', 'xmlId'],
            filter: {
                propertyId: 431
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
        'catalog.productPropertyEnum.list',
        [
            'select' => ['id', 'propertyId', 'value', 'def', 'sort', 'xmlId'],
            'filter' => ['propertyId' => 431],
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
        "productPropertyEnums": [
        {
            "def": "N",
            "id": 433,
            "propertyId": 431,
            "sort": 500,
            "value": "first",
            "xmlId": "1"
        },
        {
            "def": "N",
            "id": 435,
            "propertyId": 431,
            "sort": 500,
            "value": "second",
            "xmlId": "2"
        },
        {
            "def": "N",
            "id": 437,
            "propertyId": 431,
            "sort": 500,
            "value": "third",
            "xmlId": "3"
        },
        {
            "def": "N",
            "id": 439,
            "propertyId": 431,
            "sort": 500,
            "value": "fourth",
            "xmlId": "4"
        },
        {
            "def": "N",
            "id": 441,
            "propertyId": 431,
            "sort": 500,
            "value": "fifth",
            "xmlId": "5"
        }
        ]
    },
    "total": 5,
    "time": {
        "start": 1774339997,
        "finish": 1774339997.456867,
        "duration": 0.456866979598999,
        "processing": 0,
        "date_start": "2026-03-24T11:13:17+01:00",
        "date_finish": "2026-03-24T11:13:17+01:00",
        "operating_reset_at": 1774340597,
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
|| **productPropertyEnums**
[`catalog_product_property_enum[]`](../data-types.md#catalog_product_property_enum) | An array of objects containing information about the selected values of list properties ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. This field is returned if there are more records ||
|| **total**
[`integer`](../../data-types.md) | The total number of records. This field is not returned if the request is executed with `start = -1` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid value string to match with parameter filter. Should be value of type array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access Denied | Insufficient permissions to view the catalog ||
|| `100` | Invalid value {wrong_type} to match with parameter {filter}. Should be value of type array. | Invalid data type for the `filter` parameter value ||
|| `100` | Invalid value {wrong_type} to match with parameter {select}. Should be value of type array. | Invalid data type for the `select` parameter value ||
|| `100` | Invalid value {wrong_type} to match with parameter {order}. Should be value of type array. | Invalid data type for the `order` parameter value ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-enum-add.md)
- [{#T}](./catalog-product-property-enum-update.md)
- [{#T}](./catalog-product-property-enum-get.md)
- [{#T}](./catalog-product-property-enum-delete.md)
- [{#T}](./catalog-product-property-enum-get-fields.md)
# Get a list of product properties or variations catalog.productProperty.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the catalog

The method `catalog.productProperty.list` returns a list of product properties and variations based on the filter.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the object [catalog_product_property](../data-types.md#catalog_product_property)).

If the parameter is not provided, all fields will be selected ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected properties in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_property](../data-types.md#catalog_product_property).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol in the filter value should not be included
- `=%` — LIKE, substring search. The `%` symbol should be included in the value
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value should not be included
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

If `iblockId` is not provided, the method selects properties from all trade catalogs. If an `iblockId` is provided that is not a catalog, the method will return an empty list ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected properties in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_product_property](../data-types.md#catalog_product_property).

Possible values for `order`:
- `ASC` — in ascending order
- `DESC` — in descending order

If the parameter is not provided, sorting `id ASC` is applied ||
|| **start**
[`integer`](../../data-types.md) | The parameter is used to control pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — `100` and so on.

The formula for calculating the `start` parameter value: `start = (N - 1) * 50`, where `N` is the desired page number.

If the value `-1` is passed, the response will not include the `total` field ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"select":["id","name","iblockId","propertyType"],"filter":{"iblockId":19},"order":{"id":"ASC"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productProperty.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"select":["id","name","iblockId","propertyType"],"filter":{"iblockId":19},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productProperty.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductPropertyListResult = {
      productProperties: {
        iblockId: number
        id: number
        name: string
        propertyType: string
      }[]
    }

    try {
      // catalog.productProperty.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ProductPropertyListResult>({
        method: 'catalog.productProperty.list',
        params: {
          select: ['id', 'name', 'iblockId', 'propertyType'],
          filter: { iblockId: 19 },
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
        console.info(result.productProperties.length, result.productProperties)
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
      async function fetchProductPropertyList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.productProperty.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.productProperty.list',
            params: {
              select: ['id', 'name', 'iblockId', 'propertyType'],
              filter: { iblockId: 19 },
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
          console.info(result.productProperties.length, result.productProperties)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchProductPropertyList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productProperty.list',
                [
                    'select' => ['id', 'name', 'iblockId', 'propertyType'],
                    'filter' => [
                        'iblockId' => 19,
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
        'catalog.productProperty.list',
        {
            select: ['id', 'name', 'iblockId', 'propertyType'],
            filter: {
                iblockId: 19
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
        'catalog.productProperty.list',
        [
            'select' => ['id', 'name', 'iblockId', 'propertyType'],
            'filter' => ['iblockId' => 19],
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
        "productProperties": [
            {
                "iblockId": 19,
                "id": 115,
                "name": "Brand",
                "propertyType": "L"
            },
            {
                "iblockId": 19,
                "id": 659,
                "name": "Category",
                "propertyType": "S"
            },
            ... // description for each property
        ]
    },
    "next": 50,
    "total": 51,
    "time": {
        "start": 1773933226,
        "finish": 1773933226.400275,
        "duration": 0.40027499198913574,
        "processing": 0,
        "date_start": "2026-03-19T18:13:46+02:00",
        "date_finish": "2026-03-19T18:13:46+02:00",
        "operating_reset_at": 1773933826,
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
|| **productProperties**
[`catalog_product_property[]`](../data-types.md#catalog_product_property) | An array of objects with information about the selected properties ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. The field is returned if there are more records ||
|| **total**
[`integer`](../../data-types.md) | Total number of records. The field is not returned if the request is made with `start = -1` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Access Denied | Insufficient rights to view the catalog ||
|| `400` | `100` | Invalid value {`...`} to match with parameter {filter}. Should be value of type array | Invalid data type for the value of the `filter` parameter ||
|| `400` | `100` | Invalid value {`...`} to match with parameter {select}. Should be value of type array | Invalid data type for the value of the `select` parameter ||
|| `400` | `100` | Invalid value {`...`} to match with parameter {order}. Should be value of type array | Invalid data type for the value of the `order` parameter ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-add.md)
- [{#T}](./catalog-product-property-update.md)
- [{#T}](./catalog-product-property-get.md)
- [{#T}](./catalog-product-property-delete.md)
- [{#T}](./catalog-product-property-get-fields.md)
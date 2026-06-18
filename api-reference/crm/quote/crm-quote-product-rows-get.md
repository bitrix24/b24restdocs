# Retrieve Product Rows of the Estimate crm.quote.productrows.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "read" access permission for estimates

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.productrow.*](../universal/product-rows/index.md).

{% endnote %}

The method `crm.quote.productrows.get` returns the product rows of an estimate.

## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Identifier of the estimate.

The identifier can be obtained using the methods [crm.quote.list](./crm-quote-list.md) or [crm.quote.add](./crm-quote-add.md) ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

Retrieve product rows of the estimate with `id = 1`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.productrows.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.productrows.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each ProductRow returned in result[]
    type ProductRow = {
      ID: string
      OWNER_ID: string
      OWNER_TYPE: string
      PRODUCT_ID: number
      PRODUCT_NAME: string
      ORIGINAL_PRODUCT_NAME: string
      PRODUCT_DESCRIPTION: string
      PRICE: number
      PRICE_EXCLUSIVE: number
      PRICE_NETTO: number
      PRICE_BRUTTO: number
      PRICE_ACCOUNT: string
      QUANTITY: number
      DISCOUNT_TYPE_ID: number
      DISCOUNT_RATE: number
      DISCOUNT_SUM: number
      TAX_RATE: number
      TAX_INCLUDED: string
      CUSTOMIZED: string
      MEASURE_CODE: number
      MEASURE_NAME: string
      SORT: number
      XML_ID: string | null
      TYPE: number
      STORE_ID: number | null
      RESERVE_ID: number | null
      DATE_RESERVE_END: ISODate | null
      RESERVE_QUANTITY: number | null
    }

    try {
      const response = await $b24.actions.v2.call.make<ProductRow[]>({
        method: 'crm.quote.productrows.get',
        params: {
          id: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Product rows count:', result.length, result.map(r => r.PRODUCT_NAME))
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
      async function getQuoteProductRows() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.productrows.get',
            params: {
              id: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Product rows count:', result.length, result.map(r => r.PRODUCT_NAME))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getQuoteProductRows)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.productrows.get',
                [
                    'id' => 1,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting quote product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.productrows.get',
        {
            id: 1,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.productrows.get',
        [
            'id' => 1,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "77",
            "OWNER_ID": "1",
            "OWNER_TYPE": "Q",
            "PRODUCT_ID": 459,
            "PRODUCT_NAME": "Two-Week Subscription",
            "ORIGINAL_PRODUCT_NAME": "Two-Week Subscription",
            "PRODUCT_DESCRIPTION": "",
            "PRICE": 3000,
            "PRICE_EXCLUSIVE": 3000,
            "PRICE_NETTO": 0,
            "PRICE_BRUTTO": 0,
            "PRICE_ACCOUNT": "3000.00000000",
            "QUANTITY": 1,
            "DISCOUNT_TYPE_ID": 2,
            "DISCOUNT_RATE": 0,
            "DISCOUNT_SUM": 0,
            "TAX_RATE": 0,
            "TAX_INCLUDED": "Y",
            "CUSTOMIZED": "Y",
            "MEASURE_CODE": 796,
            "MEASURE_NAME": "pcs",
            "SORT": 10,
            "XML_ID": null,
            "TYPE": 1,
            "STORE_ID": null,
            "RESERVE_ID": null,
            "DATE_RESERVE_END": null,
            "RESERVE_QUANTITY": null
        }
    ],
    "time": {
        "start": 1773415393,
        "finish": 1773415393.257197,
        "duration": 0.25719690322875977,
        "processing": 0,
        "date_start": "2026-03-13T18:23:13+01:00",
        "date_finish": "2026-03-13T18:23:13+01:00",
        "operating_reset_at": 1773415993,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`productrow[]`](#productrow) | Root element of the response containing an array of product rows of the estimate ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

#### Type productrow {#productrow}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the product row ||
|| **OWNER_ID**
[`integer`](../data-types.md) | Identifier of the element to which the product is linked. For this method, it equals the `id` of the estimate ||
|| **OWNER_TYPE**
[`string`](../data-types.md) | String identifier of the CRM object type to which the product is linked. For this method, it is always `Q` ||
|| **PRODUCT_ID**
[`integer`](../data-types.md) | Identifier of the product in the catalog. `0` if the product is not from the catalog.

To get detailed information about the product, use [catalog.product.get](../../catalog/product/catalog-product-get.md) ||
|| **PRODUCT_NAME**
[`string`](../data-types.md) | Name of the product row ||
|| **ORIGINAL_PRODUCT_NAME**
[`string`](../data-types.md) | Name of the product row in the catalog ||
|| **PRODUCT_DESCRIPTION**
[`string`](../data-types.md) | Description of the product row ||
|| **PRICE**
[`double`](../data-types.md) | Final cost of the product per unit ||
|| **PRICE_EXCLUSIVE**
[`double`](../data-types.md) | Cost per unit considering discounts, excluding taxes ||
|| **PRICE_NETTO**
[`double`](../data-types.md) | Cost per unit excluding discounts and taxes ||
|| **PRICE_BRUTTO**
[`double`](../data-types.md) | Cost per unit excluding discounts but including taxes ||
|| **PRICE_ACCOUNT**
[`string`](../data-types.md) | Cost of the product in reporting currency ||
|| **QUANTITY**
[`double`](../data-types.md) | Quantity of product units ||
|| **DISCOUNT_TYPE_ID**
[`integer`](../data-types.md) | Type of discount:
- `1` — absolute
- `2` — percentage ||
|| **DISCOUNT_RATE**
[`double`](../data-types.md) | Discount value in percentage ||
|| **DISCOUNT_SUM**
[`double`](../data-types.md) | Absolute value of the discount ||
|| **TAX_RATE**
[`double`](../data-types.md) | Tax rate in percentage ||
|| **TAX_INCLUDED**
[`char`](../data-types.md) | Whether tax is included in the price:
- `Y` — yes
- `N` — no ||
|| **CUSTOMIZED**
[`char`](../data-types.md) | Indicator of manual modification of the product row:
- `Y` — yes
- `N` — no ||
|| **MEASURE_CODE**
[`catalog_measure.code`](../../catalog/data-types.md#catalog_measure) | Unit of measure code ||
|| **MEASURE_NAME**
[`string`](../data-types.md) | Text representation of the unit of measure ||
|| **SORT**
[`integer`](../data-types.md) | Sorting ||
|| **XML_ID**
[`string`](../data-types.md) | External code of the product row ||
|| **TYPE**
[`integer`](../data-types.md) | Type of product:
- `1` — simple product
- `4` — trade offer (variation)
- `7` — service ||
|| **STORE_ID**
[`integer`](../data-types.md) | Identifier of the warehouse ||
|| **RESERVE_ID**
[`integer`](../data-types.md) | Identifier of the reserve ||
|| **DATE_RESERVE_END**
[`date`](../data-types.md) | Date of reservation end ||
|| **RESERVE_QUANTITY**
[`double`](../data-types.md) | Quantity of reserved product units ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `The parameter id is invalid or not defined.` | The `id` parameter has no value or an invalid value was provided ||
|| `-` | `Access denied.` | The user does not have permission to read the estimate ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-product-rows-set.md)
- [{#T}](./crm-quote-get.md)
- [{#T}](./crm-quote-update.md)
- [{#T}](./crm-quote-add.md)
- [{#T}](./crm-quote-fields.md)
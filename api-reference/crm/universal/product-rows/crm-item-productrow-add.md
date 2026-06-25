# Add Product Row to CRM Object crm.item.productrow.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires access permission to modify the CRM object to which the product row is being added.

Adds a product row to a CRM object.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object containing field values for adding the product row to the CRM object ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **ownerId***
[`integer`](../../../data-types.md) | Identifier of the CRM object. ||
|| **ownerType***
[`string`](../../../data-types.md) | Identifier of the [CRM object type](../../data-types.md#object_type). Pass the [Short symbolic code of the type](../../data-types.md#object_type) ||
|| **productId**
[`catalog_product.id`](../../../catalog/data-types.md#catalog_product) | Identifier of the product from the catalog. ||
|| **productName**
[`string`](../../../data-types.md) | Name of the product in the product row. If not provided, but `productId` is given, the product name from the product catalog is used ||
|| **price**
[`double`](../../../data-types.md) | Price per unit of the product row, including discounts and taxes. ||
|| **quantity**
[`double`](../../../data-types.md) | Quantity of the product. Default is 1 ||
|| **discountTypeId**
[`integer`](../../../data-types.md) | Type of discount. Possible values:
- `1` — absolute value
- `2` — percentage value
Default is 2 ||
|| **discountRate**
[`double`](../../../data-types.md) | The discount value in percentage (if using the percentage discount type) ||
|| **discountSum**
[`double`](../../../data-types.md) | The absolute value of the discount (if using the absolute discount type) ||
|| **taxRate**
[`double`](../../../data-types.md) | Tax rate in percentage. ||
|| **taxIncluded**
[`string`](../../../data-types.md) | Indicator of whether the tax is included in the price. Possible values:
- `Y` – tax included
- `N` – tax not included
Default is N ||
|| **measureCode**
[`catalog_measure.code`](../../../catalog/data-types.md#catalog_measure) | Unit of measure code. If not provided and `productId` is given, the unit of measure from the product catalog is used ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ownerId":13142,"ownerType":"D","productId":9621,"price":80000,"quantity":2,"discountTypeId":2,"discountRate":20,"taxRate":20,"taxIncluded":"Y","measureCode":796,"sort":10}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ownerId":13142,"ownerType":"D","productId":9621,"price":80000,"quantity":2,"discountTypeId":2,"discountRate":20,"taxRate":20,"taxIncluded":"Y","measureCode":796,"sort":10},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductRowAddResult = {
      productRow: {
        id: number
        ownerId: number
        ownerType: string
        productId: number
        price: number
        quantity: number
        discountTypeId: number
        discountRate: number
        taxRate: number
        taxIncluded: string
        measureCode: number
        sort: number
        type: number
        productName: string
        priceAccount: number
        priceExclusive: number
        priceNetto: number
        priceBrutto: number
        discountSum: number
        customized: string
        measureName: string
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ProductRowAddResult>({
        method: 'crm.item.productrow.add',
        params: {
          fields: {
            ownerId: 13142,
            ownerType: 'D',
            productId: 9621,
            price: 80000,
            quantity: 2,
            discountTypeId: 2,
            discountRate: 20,
            taxRate: 20,
            taxIncluded: 'Y',
            measureCode: 796,
            sort: 10,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.productRow.id, result.productRow.productName, result.productRow.price)
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
      async function addProductRow() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.productrow.add',
            params: {
              fields: {
                ownerId: 13142,
                ownerType: 'D',
                productId: 9621,
                price: 80000,
                quantity: 2,
                discountTypeId: 2,
                discountRate: 20,
                taxRate: 20,
                taxIncluded: 'Y',
                measureCode: 796,
                sort: 10,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.productRow.id, result.productRow.productName, result.productRow.price)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addProductRow)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.productrow.add',
                [
                    'fields' => [
                        'ownerId'        => 13142,
                        'ownerType'      => 'D',
                        'productId'      => 9621,
                        'price'          => 80000.000000,
                        'quantity'       => 2,
                        'discountTypeId' => 2,
                        'discountRate'   => 20,
                        'taxRate'        => 20,
                        'taxIncluded'    => 'Y',
                        'measureCode'    => 796,
                        'sort'           => 10,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding product row: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.item.productrow.add(
            fields={
                "ownerId": 13142,
                "ownerType": "D",
                "productId": 9621,
                "price": 80000,
                "quantity": 2,
                "discountTypeId": 2,
                "discountRate": 20,
                "taxRate": 20,
                "taxIncluded": "Y",
                "measureCode": 796,
                "sort": 10,
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.productrow.add', {
            fields: {
                ownerId: 13142,
                ownerType: 'D',
                productId: 9621,
                price: 80000.000000,
                quantity: 2,
                discountTypeId: 2,
                discountRate: 20,
                taxRate: 20,
                taxIncluded: 'Y',
                measureCode: 796,
                sort: 10,
            },
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
        'crm.item.productrow.add',
        [
            'fields' => [
                'ownerId' => 13142,
                'ownerType' => 'D',
                'productId' => 9621,
                'price' => 80000.000000,
                'quantity' => 2,
                'discountTypeId' => 2,
                'discountRate' => 20,
                'taxRate' => 20,
                'taxIncluded' => 'Y',
                'measureCode' => 796,
                'sort' => 10,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response on Success

HTTP status: **200**

```json
{
   "result":{
      "productRow":{
         "id":17647,
         "ownerId":13142,
         "ownerType":"D",
         "productId":9621,
         "price":80000,
         "quantity":2,
         "discountTypeId":2,
         "discountRate":20,
         "taxRate":20,
         "taxIncluded":"Y",
         "measureCode":796,
         "sort":10,
         "type":4,
         "productName":"iphone 14",
         "priceAccount":80000,
         "priceExclusive":66666.67,
         "priceNetto":83333.34,
         "priceBrutto":100000.01,
         "discountSum":16666.67,
         "customized":"Y",
         "measureName":"pcs",
         "xmlId":""
      }
   },
   "time":{
      "start":1716887721.77879,
      "finish":1716887723.259695,
      "duration":1.4809050559997559,
      "processing":1.2986550331115723,
      "date_start":"2024-05-28T12:15:21+03:00",
      "date_finish":"2024-05-28T12:15:23+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **productRow**
[`crm_item_product_row`](../../data-types.md#crm_item_product_row) | Object containing information about the added product row ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":"OWNER_NOT_FOUND",
   "error_description":"Owner was not found"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | Working with this type of objects is not supported ||
|| `ACCESS_DENIED` | Access denied ||
|| `OWNER_NOT_FOUND` | The provided CRM object was not found ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-fields.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-set.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
- [{#T}](./crm-item-productrow-list.md)
- [{#T}](./crm-item-productrow-delete.md)
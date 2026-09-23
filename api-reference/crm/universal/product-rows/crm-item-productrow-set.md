# Save Product Rows of CRM Object crm.item.productrow.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: access permission to modify the CRM object whose product rows are saved is required.

Saves the set of product rows of a CRM object.

{% note warning "" %}

The method replaces all product rows of the CRM object with the set that is passed. Rows that are absent from `productRows` are deleted together with their items in [payments](../payment/products-in-payment/index.md), and the total of the CRM object is recalculated. If an empty array is passed, the CRM object is left with no product rows.

To add a single row without changing the rest, use the [crm.item.productrow.add](./crm-item-productrow-add.md) method.

{% endnote %}

The set is saved as a whole: if at least one row fails validation, the method returns the `INVALID_ARG_VALUE` error and leaves the product rows of the CRM object unchanged.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ownerId***
[`integer`](../../../data-types.md) | Identifier of the CRM object. ||
|| **ownerType***
[`string`](../../../data-types.md) | Short symbolic code of the [CRM object type](../../data-types.md#object_type): `L` — lead, `D` — deal, `Q` — estimate, `SI` — invoice (new), `T` followed by the hexadecimal type identifier — SPA ||
|| **productRows***
[`object[]`](../../../data-types.md) | Array of objects containing information about the product rows to be saved in the object [(detailed description)](#productRows) ||
|#


### Parameter productRows {#productRows}

In every row, pass either `productId` (identifier of a product from the catalog) or `productName` (name of an arbitrary row).

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`crm_item_product_row.id`](../../data-types.md#crm_item_product_row) | Identifier of an existing product row. It can be retrieved with the [crm.item.productrow.list](./crm-item-productrow-list.md) method.
Pass `id` to save the row under the same identifier. Without `id`, the row is created anew.
If `id` of a non-existent row or of a row of another CRM object is passed, the method creates a new row and returns no error ||
|| **productId**
[`catalog_product.id`](../../../catalog/data-types.md#catalog_product) | Identifier of the product from the catalog.
If it is not provided, a row with no link to the catalog is created ||
|| **productName**
[`string`](../../../data-types.md) | Name of the product in the product row.
If it is not provided, but `productId` is given, the product name from the product catalog is used ||
|| **price**
[`double`](../../../data-types.md) | Price per unit of the product row, including discounts and taxes. It is specified in the currency of the CRM object.
If it is not provided, the price equals `0` — even when `productId` with a catalog price is passed ||
|| **quantity**
[`double`](../../../data-types.md) | Quantity of the product.
Default is `1` ||
|| **discountTypeId**
[`integer`](../../../data-types.md) | Type of discount. Possible values:
- `1` — absolute value
- `2` — percentage value
Default is `2` ||
|| **discountRate**
[`double`](../../../data-types.md) | The discount value in percentage (if using the percentage discount type) ||
|| **discountSum**
[`double`](../../../data-types.md) | The absolute value of the discount (if using the absolute discount type) ||
|| **taxRate**
[`double`](../../../data-types.md) | Tax rate in percentage. ||
|| **taxIncluded**
[`string`](../../../data-types.md) | Indicator of whether the tax is included in the price. Possible values:
- `Y` — tax included
- `N` — tax not included
Default is `N` ||
|| **taxName**
[`string`](../../../data-types.md) | Name of the tax rate. For example, `VAT 20`.
It is a label only: the calculation uses the `taxRate` value, and the method retains `taxName` as is. If `taxName` is not passed, a row without tax gets the `No VAT` value ||
|| **measureCode**
[`catalog_measure.code`](../../../catalog/data-types.md#catalog_measure) | Unit of measurement code.
If it is not provided, but `productId` is given, the unit of measurement from the product catalog is used ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting ||
|#

The method calculates the `priceAccount`, `priceExclusive`, `priceNetto`, `priceBrutto`, `measureName`, `type`, `customized`, `xmlId`, and `storeId` fields on its own. The method ignores values passed for these fields without raising an error: sending back a row retrieved with the [crm.item.productrow.list](./crm-item-productrow-list.md) method does not change them.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerType":"D","ownerId":13143,"productRows":[{"productId":9621,"price":99999.99,"quantity":1,"sort":10},{"productId":9623,"price":15900,"quantity":2,"sort":10}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.set
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ownerType":"D","ownerId":13143,"productRows":[{"productId":9621,"price":99999.99,"quantity":1,"sort":10},{"productId":9623,"price":15900,"quantity":2,"sort":10}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.set
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductRowsResult = {
      productRows: {
        id: number
        ownerId: number
        ownerType: string
        productId: number
        productName: string
        price: number
        priceAccount: number
        priceExclusive: number
        priceNetto: number
        priceBrutto: number
        quantity: number
        discountTypeId: number
        discountRate: number
        discountSum: number
        taxRate: number | null
        taxIncluded: string
        taxName: string
        customized: string
        measureCode: number
        measureName: string
        sort: number
        xmlId: string
        type: number
      }[]
    }

    try {
      const response = await $b24.actions.v2.call.make<ProductRowsResult>({
        method: 'crm.item.productrow.set',
        params: {
          ownerType: 'D',
          ownerId: 13143,
          productRows: [
            {
              productId: 9621,
              price: 99999.99,
              quantity: 1,
              sort: 10,
            },
            {
              productId: 9623,
              price: 15900,
              quantity: 2,
              sort: 10,
            },
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.productRows)
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
      async function setProductRows() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.productrow.set',
            params: {
              ownerType: 'D',
              ownerId: 13143,
              productRows: [
                {
                  productId: 9621,
                  price: 99999.99,
                  quantity: 1,
                  sort: 10,
                },
                {
                  productId: 9623,
                  price: 15900,
                  quantity: 2,
                  sort: 10,
                },
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.productRows)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setProductRows)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.productrow.set(
            owner_type="D",
            owner_id=13143,
            product_rows=[
                {
                    "productId": 9621,
                    "price": 99999.99,
                    "quantity": 1,
                    "sort": 10,
                },
                {
                    "productId": 9623,
                    "price": 15900,
                    "quantity": 2,
                    "sort": 10,
                },
            ],
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.productrow.set',
                [
                    'ownerType'   => 'D',
                    'ownerId'     => 13143,
                    'productRows' => [
                        [
                            'productId' => 9621,
                            'price'     => 99999.99,
                            'quantity'  => 1,
                            'sort'      => 10,
                        ],
                        [
                            'productId' => 9623,
                            'price'     => 15900,
                            'quantity'  => 2,
                            'sort'      => 10,
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.productrow.set', {
            ownerType: 'D',
            ownerId: 13143,
            productRows: [{
                    productId: 9621,
                    price: 99999.99,
                    quantity: 1,
                    sort: 10,
                },
                {
                    productId: 9623,
                    price: 15900,
                    quantity: 2,
                    sort: 10,
                },

            ],
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
        'crm.item.productrow.set',
        [
            'ownerType' => 'D',
            'ownerId' => 13143,
            'productRows' => [
                [
                    'productId' => 9621,
                    'price' => 99999.99,
                    'quantity' => 1,
                    'sort' => 10,
                ],
                [
                    'productId' => 9623,
                    'price' => 15900,
                    'quantity' => 2,
                    'sort' => 10,
                ]
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
    res, err := client.Core().Call(ctx, "crm.item.productrow.set", b24.Params{
    	"ownerType": "D",
    	"ownerId":   13143,
    	"productRows": []b24.Params{
    		{
    			"productId": 9621,
    			"price":     99999.99,
    			"quantity":  1,
    			"sort":      10,
    		},
    		{
    			"productId": 9623,
    			"price":     15900,
    			"quantity":  2,
    			"sort":      10,
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.item.productrow.set: %w", err)
    }

    // The method wraps the response in an object with the "productRows" key.
    raw, ok := b24.Unwrap(res.Result, "productRows")
    if !ok {
    	return fmt.Errorf("no productRows key in the response")
    }

    var items []struct {
    	ID          b24.ID  `json:"id"`
    	OwnerID     b24.ID  `json:"ownerId"`
    	OwnerType   string  `json:"ownerType"`
    	ProductID   b24.ID  `json:"productId"`
    	ProductName string  `json:"productName"`
    	Price       float64 `json:"price"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID)
    }
    ```

{% endlist %}

## Response on Success

HTTP status: **200**

```json
{
   "result":{
      "productRows":[
         {
            "id":17654,
            "ownerId":13143,
            "ownerType":"D",
            "productId":9621,
            "productName":"iphone 14",
            "price":99999.99,
            "priceAccount":99999.99,
            "priceExclusive":99999.99,
            "priceNetto":99999.99,
            "priceBrutto":99999.99,
            "quantity":1,
            "discountTypeId":2,
            "discountRate":0,
            "discountSum":0,
            "taxRate":null,
            "taxIncluded":"N",
            "taxName":"No VAT",
            "customized":"Y",
            "measureCode":796,
            "measureName":"pcs",
            "sort":10,
            "xmlId":"",
            "type":4
         },
         {
            "id":17655,
            "ownerId":13143,
            "ownerType":"D",
            "productId":9623,
            "productName":"iphone 10xs",
            "price":15900,
            "priceAccount":15900,
            "priceExclusive":15900,
            "priceNetto":15900,
            "priceBrutto":15900,
            "quantity":2,
            "discountTypeId":2,
            "discountRate":0,
            "discountSum":0,
            "taxRate":null,
            "taxIncluded":"N",
            "taxName":"No VAT",
            "customized":"Y",
            "measureCode":796,
            "measureName":"pcs",
            "sort":10,
            "xmlId":"",
            "type":4
         }
      ]
   },
   "time":{
      "start":1716895718.887229,
      "finish":1716895719.316293,
      "duration":0.4290640354156494,
      "processing":0.20114707946777344,
      "date_start":"2024-05-28T14:28:38+03:00",
      "date_finish":"2024-05-28T14:28:39+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### The result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **productRows**
[`crm_item_product_row[]`](../../data-types.md#crm_item_product_row) | Array of objects with information about all product rows of the CRM object after saving. The method returns them all at once, with no pagination ||
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
|| `ENTITY_TYPE_NOT_SUPPORTED` | This type of CRM object does not support product rows ||
|| `ACCESS_DENIED` | The user has no permission to modify the CRM object ||
|| `OWNER_NOT_FOUND` | The provided CRM object was not found ||
|| `INVALID_ARG_VALUE` | The product with the provided `productId` was not found in the catalog ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-add.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-list.md)
- [{#T}](./crm-item-productrow-delete.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
- [{#T}](./crm-item-productrow-fields.md)

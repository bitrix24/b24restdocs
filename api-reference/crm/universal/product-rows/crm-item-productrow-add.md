# Add Product Row to CRM Object crm.item.productrow.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires access permission to modify the CRM object to which the product row is being added.

Adds a product row to a CRM object.

The method adds a single row and does not change the rest. To replace the whole set of product rows of a CRM object, use the [crm.item.productrow.set](./crm-item-productrow-set.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object containing field values for adding the product row to the CRM object [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

Pass either `productId` (identifier of a product from the catalog) or `productName` (name of an arbitrary row). The full list of fields with their types and write availability is returned by the [crm.item.productrow.fields](./crm-item-productrow-fields.md) method.

#|
|| **Name**
`type` | **Description** ||
|| **ownerId***
[`integer`](../../../data-types.md) | Identifier of the CRM object. ||
|| **ownerType***
[`string`](../../../data-types.md) | Short symbolic code of the [CRM object type](../../data-types.md#object_type): `L` — lead, `D` — deal, `Q` — estimate, `SI` — invoice (new), `T` followed by the hexadecimal type identifier — SPA ||
|| **productId**
[`catalog_product.id`](../../../catalog/data-types.md#catalog_product) | Identifier of the product from the catalog.
If it is not provided, a row with no link to the catalog is created ||
|| **productName**
[`string`](../../../data-types.md) | Name of the product in the product row. If not provided, but `productId` is given, the product name from the product catalog is used ||
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
[`catalog_measure.code`](../../../catalog/data-types.md#catalog_measure) | Unit of measure code.
If it is not provided, but `productId` is given, the unit of measure from the product catalog is used ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting ||
|#

The method calculates the `id`, `priceAccount`, `priceExclusive`, `priceNetto`, `priceBrutto`, `measureName`, `type`, `customized`, `xmlId`, and `storeId` fields on its own. The method ignores values passed for these fields without raising an error.

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
        taxRate: number | null
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

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

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
                        'price'          => 80000,
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

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.productrow.add', {
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
                'price' => 80000,
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.item.productrow.add", b24.Params{
    	"fields": b24.Params{
    		"ownerId":        13142,
    		"ownerType":      "D",
    		"productId":      9621,
    		"price":          80000,
    		"quantity":       2,
    		"discountTypeId": 2,
    		"discountRate":   20,
    		"taxRate":        20,
    		"taxIncluded":    "Y",
    		"measureCode":    796,
    		"sort":           10,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.item.productrow.add: %w", err)
    }

    // The method wraps the response in an object with the "productRow" key.
    raw, ok := b24.Unwrap(res.Result, "productRow")
    if !ok {
    	return fmt.Errorf("no productRow key in the response")
    }

    var item struct {
    	ID        b24.ID  `json:"id"`
    	OwnerID   b24.ID  `json:"ownerId"`
    	OwnerType string  `json:"ownerType"`
    	ProductID b24.ID  `json:"productId"`
    	Price     float64 `json:"price"`
    	Quantity  float64 `json:"quantity"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.OwnerID)
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
         "priceExclusive":66666.66666667,
         "priceNetto":83333.33333333,
         "priceBrutto":100000,
         "discountSum":16666.66666667,
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
[`object`](../../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### The result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **productRow**
[`crm_item_product_row`](../../data-types.md#crm_item_product_row) | Object with information about the added product row ||
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
|| `ACCESS_DENIED` | The user has no permission to modify the CRM object the row is added to ||
|| `OWNER_NOT_FOUND` | The provided CRM object was not found ||
|| `INVALID_ARG_VALUE` | The product with the provided `productId` was not found in the catalog ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-list.md)
- [{#T}](./crm-item-productrow-delete.md)
- [{#T}](./crm-item-productrow-set.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
- [{#T}](./crm-item-productrow-fields.md)

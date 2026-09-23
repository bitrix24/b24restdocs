# Get Product Rows of the CRM Object crm.item.productrow.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires read access permission for the CRM object whose product rows are being selected

Retrieves product rows of a CRM object.

If a single row with a known identifier is needed, use the [crm.item.productrow.get](./crm-item-productrow-get.md) method. If only the rows with no payment issued are needed, use the [crm.item.productrow.getAvailableForPayment](./crm-item-productrow-get-available-for-payment.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../../data-types.md) | Object for filtering selected records in the format `{"field_1": "value_1", ... "field_N": "value_N"}` [(detailed description)](#filter) ||
|| **order**
[`object`](../../../data-types.md) | Object for sorting selected product rows in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Sorting is available by the `id` and `sort` fields. The method ignores the other fields.

Without the `order` parameter, the method returns the rows in ascending order of the `sort` value.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static: 50 records.

To select the second page of results, pass the value `50`, the third one — `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number.

Pass `-1` to skip counting the total number of records: the method does not return `total` and works faster.

Default is `0`
 ||
|#


The method does not support the `select` parameter and always returns the full set of product row fields.

### Parameter filter {#filter}

The `=ownerType` and `=ownerId` keys are required — without them the method returns the `REQUIRED_ARG_MISSING` error.

#|
|| **Name**
`type` | **Description** ||
|| **=ownerType***
[`string`](../../../data-types.md) | Short symbolic code of the [CRM object type](../../data-types.md#object_type): `L` — lead, `D` — deal, `Q` — estimate, `SI` — invoice (new), `T` followed by the hexadecimal type identifier — SPA ||
|| **=ownerId***
[`integer`](../../../data-types.md) | Identifier of the CRM object ||
|| **id**
[`crm_item_product_row.id`](../../data-types.md#crm_item_product_row) | Identifier of the product row ||
|| **productId**
[`catalog_product.id`](../../../catalog/data-types.md#catalog_product) | Identifier of the product from the catalog ||
|#

Filtering is available only by the fields from the table: the other fields of a product row have no index in the database. The method silently ignores conditions on `price`, `productName`, `quantity`, `sort`, `type`, and on unknown fields and returns all product rows of the CRM object — check the composition of the selection on your side.

The key may have an additional prefix that specifies the behavior of the filter. Possible prefix values:

- `=` — equals
- `!=` — not equal
- `>` — greater than
- `>=` — greater than or equal to
- `<` — less than
- `<=` — less than or equal to
- `@` — any of the values of an array

The prefixes apply to the `id` and `productId` keys. Pass the `=ownerType` and `=ownerId` keys with the `=` prefix only.

The only key that accepts an array of values is `@id`. For example, `{"@id": [17640, 17641]}` returns two rows. In the other keys, the method ignores an array in the same way as an unsupported condition.

Write the filter keys in camelCase: `=ownerType`, not `=OWNER_TYPE`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"=ownerType":"D","=ownerId":13142,">id":17640},"order":{"sort":"asc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"=ownerType":"D","=ownerId":13142,">id":17640},"order":{"sort":"asc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductRowListResult = {
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
        storeId: number | null
      }[]
    }

    try {
      // crm.item.productrow.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ProductRowListResult>({
        method: 'crm.item.productrow.list',
        params: {
          filter: {
            '=ownerType': 'D',
            '=ownerId': 13142,
            '>id': 17640,
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
        console.info(result.productRows.length, result.productRows)
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
      async function listProductRows() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.item.productrow.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.productrow.list',
            params: {
              filter: {
                '=ownerType': 'D',
                '=ownerId': 13142,
                '>id': 17640,
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
          console.info(result.productRows.length, result.productRows)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listProductRows)
    </script>
    ```

- Python

    Example

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.productrow.list(
            filter={
                "=ownerType": "D",
                "=ownerId": 13142,
                ">id": 17640,
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

    Example `as_list`

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.productrow.list(
            filter={
                "=ownerType": "D",
                "=ownerId": 13142,
                ">id": 17640,
            },
            order={
                "sort": "asc",
            },
            start=0,
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
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

    Example `as_list_fast`

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        # as_list_fast sets the order and the pagination on its own,
        # so order and start do not need to be passed in such a call
        bitrix_response = client.crm.item.productrow.list(
            filter={
                "=ownerType": "D",
                "=ownerId": 13142,
            },
        ).as_list_fast(descending=True).response
        result = bitrix_response.result
        for item in result:
            print(item)
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
                'crm.item.productrow.list',
                [
                    'filter' => [
                        "=ownerType" => 'D',
                        "=ownerId"   => 13142,
                        ">id"        => 17640,
                    ],
                    'order'  => [
                        'sort' => "asc"
                    ],
                    'start'  => 0,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing product rows: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.productrow.list', {
            filter: {
                "=ownerType": 'D',
                "=ownerId": 13142,
                ">id": 17640,
            },
            order: {
                sort: "asc"
            },
            start: 0,
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
        'crm.item.productrow.list',
        [
            'filter' => [
                "=ownerType" => 'D',
                "=ownerId" => 13142,
                ">id" => 17640,
            ],
            'order' => [
                'sort' => "asc"
            ],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.item.productrow.list", b24.Params{
    	"filter": b24.Params{
    		"=ownerType": "D",
    		"=ownerId":   13142,
    		">id":        17640,
    	},
    	"order": b24.Params{
    		"sort": "asc",
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.item.productrow.list: %w", err)
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
            "id":17649,
            "ownerId":13142,
            "ownerType":"D",
            "productId":9621,
            "productName":"iphone 14",
            "price":90000,
            "priceAccount":90000,
            "priceExclusive":81818.18181818,
            "priceNetto":90909.09090909,
            "priceBrutto":100000,
            "quantity":3,
            "discountTypeId":2,
            "discountRate":10,
            "discountSum":9090.90909091,
            "taxRate":10,
            "taxIncluded":"Y",
            "taxName":"VAT 10",
            "customized":"Y",
            "measureCode":796,
            "measureName":"pcs",
            "sort":10,
            "xmlId":"sale_basket_8147",
            "type":4,
            "storeId":19
         },
         {
            "id":17650,
            "ownerId":13142,
            "ownerType":"D",
            "productId":9623,
            "productName":"iphone 10xs",
            "price":5550,
            "priceAccount":5550,
            "priceExclusive":5550,
            "priceNetto":5550,
            "priceBrutto":5550,
            "quantity":1,
            "discountTypeId":2,
            "discountRate":0,
            "discountSum":0,
            "taxRate":null,
            "taxIncluded":"Y",
            "taxName":"No VAT",
            "customized":"Y",
            "measureCode":6,
            "measureName":"m",
            "sort":20,
            "xmlId":"sale_basket_8148",
            "type":4,
            "storeId":17
         }
      ]
   },
   "total":2,
   "time":{
      "start":1716905609.186602,
      "finish":1716905609.434087,
      "duration":0.24748492240905762,
      "processing":0.06894516944885254,
      "date_start":"2024-05-28T17:13:29+03:00",
      "date_finish":"2024-05-28T17:13:29+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **next**
[`integer`](../../../data-types.md) | Value for the `start` parameter of the next page. It is returned only if not all the records have been selected ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records found. It is not returned if `start: -1` is passed ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### The result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **productRows**
[`crm_item_product_row[]`](../../data-types.md#crm_item_product_row) | Array of objects with information about the selected product rows of the CRM object ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":"ACCESS_DENIED",
   "error_description":"Access denied"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `REQUIRED_ARG_MISSING` | The required `=ownerType` or `=ownerId` key is missing from the filter ||
|| `ACCESS_DENIED` | Access denied. The method returns the same error if the CRM object with the provided `=ownerId` does not exist or its type does not support product rows ||
|| `INVALID_ARG_VALUE` | Invalid values for input parameters. For example, an array in the value of a filter key with a comparison prefix ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-add.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-delete.md)
- [{#T}](./crm-item-productrow-set.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
- [{#T}](./crm-item-productrow-fields.md)

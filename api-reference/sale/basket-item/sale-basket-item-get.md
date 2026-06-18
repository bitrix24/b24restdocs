# Get Information About a Basket Item (Position) sale.basketitem.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: store manager

This method retrieves information about a basket item (position).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_basket_item.id`](../data-types.md) | Identifier of the basket item (position).

Can be obtained using the [sale.basketitem.list](./sale-basket-item-list.md) method
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6801}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketitem.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6801,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GetBasketItemResult = {
      basketItem: {
        basePrice: number
        canBuy: string
        catalogXmlId: string
        currency: string
        customPrice: string
        dateInsert: ISODate | null
        dateUpdate: ISODate | null
        dimensions: string
        discountPrice: number
        id: number
        measureCode: string
        measureName: string
        name: string
        orderId: number
        price: number
        productId: number
        productXmlId: string
        properties: unknown[]
        quantity: number
        reservations: unknown[]
        sort: number
        vatIncluded: string
        vatRate: number | null
        weight: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<GetBasketItemResult>({
        method: 'sale.basketitem.get',
        params: {
          id: 6801,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.basketItem.id, result.basketItem.name, result.basketItem.price)
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
      async function getBasketItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.basketitem.get',
            params: {
              id: 6801,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.basketItem.id, result.basketItem.name, result.basketItem.price)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getBasketItem)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.basketitem.get',
                [
                    'id' => 6801,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting basket item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.basketitem.get",
        {
            id: 6801
        },
    )
        .then(
            function(result)
            {
                if (result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    console.log(result.data());
                }
            },
            function(error)
            {
                console.info(error);
            }
        );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.basketitem.get',
        [
            'id' => 6801
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
    "result": {
        "basketItem": {
            "basePrice": 1000,
            "canBuy": "Y",
            "catalogXmlId": "FUTURE-QUICKBOOKS-CATALOG",
            "currency": "USD",
            "customPrice": "N",
            "dateInsert": "2024-04-23T15:59:37+02:00",
            "dateUpdate": "2024-04-23T15:59:37+02:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";N;s:6:\"HEIGHT\";N;s:6:\"LENGTH\";N;}",
            "discountPrice": 100,
            "id": 6801,
            "measureCode": "163",
            "measureName": "g",
            "name": "Product",
            "orderId": 5147,
            "price": 900,
            "productId": 1245,
            "productXmlId": "1245",
            "properties": [],
            "quantity": 1,
            "reservations": [],
            "sort": 100,
            "vatIncluded": "N",
            "vatRate": null,
            "weight": 0,
            "xmlId": "bx_6627bec8c4fdc"
        }
    },
    "time": {
        "start": 1713880776.108755,
        "finish": 1713880777.704221,
        "duration": 1.595465898513794,
        "processing": 0.973701000213623,
        "date_start": "2024-04-23T15:59:36+02:00",
        "date_finish": "2024-04-23T15:59:37+02:00",
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
|| **basketItem**
[`sale_basket_item`](../data-types.md) | Object containing the data of the basket item (position) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200140400001` | `basket item does not exist`

Basket position not found
|| 
|| `200040300010` | Insufficient permissions to read
|| 
|| `100` | Required parameters not specified
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-basket-item-add.md)
- [{#T}](./sale-basket-item-update.md)
- [{#T}](./sale-basket-item-list.md)
- [{#T}](./sale-basket-item-delete.md)
- [{#T}](./sale-basket-item-get-fields.md)
- [{#T}](./sale-basket-item-add-catalog-product.md)
- [{#T}](./sale-basket-item-update-catalog-product.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)

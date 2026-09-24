# Create a property for a basket item sale.basketproperties.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.basketproperties.add` adds a property for an item (position) in the basket of an order.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for creating a property of a basket item (position):

```js
fields: {
    basketId: "value",
    name: "value",
    value: "value",
    code: "value",
    sort: "value",
    xmlId: "value",
}
```
 ||
|#

### Parameter fields {#parametr-fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

The `name`, `value`, `code`, and `xmlId` fields store up to 255 characters. Bitrix24 truncates a longer value without an error.

#|
|| **Name**
`type` | **Description** ||
|| **basketId***
[`sale_basket_item.id`](../data-types.md) | Identifier of the basket item (position) in the order. The item must belong to an order: its `orderId` field is filled in.
Can be obtained using the methods [`sale.basketitem.get`](../basket-item/sale-basket-item-get.md) or [`sale.basketitem.list`](../basket-item/sale-basket-item-list.md) ||
|| **name***
[`string`](../../data-types.md) | Property name ||
|| **value***
[`string`](../../data-types.md) | Property value ||
|| **code***
[`string`](../../data-types.md) | Symbolic code of the property. Bitrix24 does not check the code for uniqueness: calling the method again with the same `code` creates a second property for the item ||
|| **sort**
[`integer`](../../data-types.md) | Position in the list of properties.
If not specified, a default value of 100 will be assigned ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the property.
If not specified, it will be generated automatically ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"basketId":6806,"name":"Article","value":"4653-4877","code":"ARTICUL"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketproperties.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"basketId":6806,"name":"Article","value":"4653-4877","code":"ARTICUL"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketproperties.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BasketPropertyAddResult = {
      basketProperty: {
        basketId: number
        code: string
        id: number
        name: string
        value: string
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<BasketPropertyAddResult>({
        method: 'sale.basketproperties.add',
        params: {
          fields: {
            basketId: 6806,
            name: 'SKU',
            value: '4653-4877',
            code: 'ARTICUL',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.basketProperty)
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
      async function addBasketProperty() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.basketproperties.add',
            params: {
              fields: {
                basketId: 6806,
                name: 'SKU',
                value: '4653-4877',
                code: 'ARTICUL',
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
          console.info(result.basketProperty)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addBasketProperty)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "basketId": 6806,
        "name": "SKU",
        "value": "4653-4877",
        "code": "ARTICUL",
    }

    try:
        bitrix_response = client.sale.basketproperties.add(
            fields=fields,
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
                'sale.basketproperties.add',
                [
                    'fields' => [
                        'basketId' => 6806,
                        'name'     => 'Article',
                        'value'    => '4653-4877',
                        'code'     => 'ARTICUL',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding basket property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.basketproperties.add",
        {
            fields: {
                basketId: 6806,
                name: 'Article',
                value: '4653-4877',
                code: 'ARTICUL',
            }
        },
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
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.basketproperties.add',
        [
            'fields' =>
            [
                'basketId' => 6806,
                'name' => 'Article',
                'value' => '4653-4877',
                'code' => 'ARTICUL',
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
    res, err := client.Core().Call(ctx, "sale.basketproperties.add", b24.Params{
    	"fields": b24.Params{
    		"basketId": 6806,
    		"name":     "Article",
    		"value":    "4653-4877",
    		"code":     "ARTICUL",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.basketproperties.add: %w", err)
    }

    // The method wraps the response in an object with the "basketProperty" key.
    raw, ok := b24.Unwrap(res.Result, "basketProperty")
    if !ok {
    	return fmt.Errorf("no basketProperty key in the response")
    }

    var item struct {
    	BasketID b24.ID `json:"basketId"`
    	Code     string `json:"code"`
    	ID       b24.ID `json:"id"`
    	Name     string `json:"name"`
    	Value    string `json:"value"`
    	XmlID    string `json:"xmlId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.BasketID, item.Code)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "basketProperty": {
            "basketId": 6806,
            "code": "ARTICUL",
            "id": 17,
            "name": "Article",
            "value": "4653-4877",
            "xmlId": "bx_662a44cff2b81"
        }
    },
    "time": {
        "start": 1714046159.109796,
        "finish": 1714046163.282623,
        "duration": 4.1728270053863525,
        "processing": 3.4128189086914062,
        "date_start": "2024-04-25T13:55:59+02:00",
        "date_finish": "2024-04-25T13:56:03+02:00",
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
|| **basketProperty**
[`sale_basket_item_property`](../data-types.md#sale_basket_item_property) | Object with data of the created property of the basket item (position) ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "200240400002",
    "error_description": "Basket item not exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | `Required fields: basketId` — the required field `basketId`, `name`, `value`, or `code` is not passed. The names of the missing fields are listed in `error_description` ||
|| `200240400004` | `Basket item id is absent` — `null` is passed in `basketId` ||
|| `200240400005` | `Basket item id is bad` — `basketId` is not a number or is less than 1, for example `"abc"` or `0` ||
|| `200240400002`, `200240400001` | `Basket item not exists` — there is no basket item with this `basketId` ||
|| `MAIN_CONTROLLER_22001` | `Argument 'id' is null or empty` — the basket item does not belong to an order: its `orderId` field is empty ||
|| `100` | `Could not find value for parameter {fields}` — the `fields` parameter is not passed ||
|| `200040300020` | `Access Denied` — insufficient permissions to add ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-basket-properties-update.md)
- [{#T}](./sale-basket-properties-get.md)
- [{#T}](./sale-basket-properties-list.md)
- [{#T}](./sale-basket-properties-delete.md)
- [{#T}](./sale-basket-properties-get-fields.md)

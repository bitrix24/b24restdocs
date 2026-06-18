# Change the property of the basket item sale.basketproperties.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method modifies the property for an item (position) in the basket of an order.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_basket_item_property.id`](../data-types.md#sale_basket_item_property) | Identifier of the order item ||
|| **fields***
[`object`](../../data-types.md) | Values of the fields to be modified (detailed description is provided [below](#parametr-fields)) for the basket item (position) property:

```js
fields: {
    name: "value",
    value: "value",
    code: "value",
    sort: "value",
    xmlId: "value",
}
```
 ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Name of the property ||
|| **value**
[`string`](../../data-types.md) | Value of the property ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the property ||
|| **sort**
[`integer`](../../data-types.md) | Position in the list of properties ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the property ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17,"fields":{"name":"Article","value":"123-456-789","code":"ARTICUL"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketproperties.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17,"fields":{"name":"Article","value":"123-456-789","code":"ARTICUL"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketproperties.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BasketPropertyUpdateResult = {
      basketProperty: {
        basketId: number
        code: string
        id: number
        name: string
        sort: number
        value: string
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<BasketPropertyUpdateResult>({
        method: 'sale.basketproperties.update',
        params: {
          id: 17,
          fields: {
            name: 'Articul',
            value: '123-456-789',
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
      async function updateBasketProperty() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.basketproperties.update',
            params: {
              id: 17,
              fields: {
                name: 'Articul',
                value: '123-456-789',
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

      document.addEventListener('DOMContentLoaded', updateBasketProperty)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.basketproperties.update',
                [
                    'id' => 17,
                    'fields' => [
                        'name'  => 'Article',
                        'value' => '123-456-789',
                        'code'  => 'ARTICUL',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating basket properties: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.basketproperties.update",
        {
            id: 17,
            fields: {
                name: 'Article',
                value: '123-456-789',
                code: 'ARTICUL',
            }
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
                    console.log(result);
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
        'sale.basketproperties.update',
        [
            'id' => 17,
            'fields' =>
            [
                'name' => 'Article',
                'value' => '123-456-789',
                'code' => 'ARTICUL',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
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
            "sort": 100,
            "value": "123-456-789",
            "xmlId": "bx_662a44cff2b81"
        }
    },
    "total": 1,
    "time": {
        "start": 1714049553.754992,
        "finish": 1714049555.158799,
        "duration": 1.4038069248199463,
        "processing": 0.67576003074646,
        "date_start": "2024-04-25T14:52:33+02:00",
        "date_finish": "2024-04-25T14:52:35+02:00",
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
[`sale_basket_item_property`](../data-types.md#sale_basket_item_property) | Object with data of the modified basket item (position) property ||
|| **total**
[`integer`](../../data-types.md) | Number of processed records ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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
|| `20004030001` | Insufficient rights to modify ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., missing required fields) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-basket-properties-add.md)
- [{#T}](./sale-basket-properties-get.md)
- [{#T}](./sale-basket-properties-list.md)
- [{#T}](./sale-basket-properties-delete.md)
- [{#T}](./sale-basket-properties-get-fields.md)

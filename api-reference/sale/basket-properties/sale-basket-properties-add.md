# Create a property for a basket item sale.basketproperties.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a property for an item (position) in the basket of an order.

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

#|
|| **Name**
`type` | **Description** ||
|| **basketId***
[`sale_basket_item.id`](../data-types.md) | Identifier of the basket item (position) in the order.
Can be obtained using the methods [`sale.basketitem.get`](../basket-item/sale-basket-item-get.md) or [`sale.basketitem.list`](../basket-item/sale-basket-item-list.md) ||
|| **name***
[`string`](../../data-types.md) | Property name ||
|| **value***
[`string`](../../data-types.md) | Property value ||
|| **code***
[`string`](../../data-types.md) | Symbolic code of the property ||
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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.basketproperties.add",
    		{
    			fields: {
    				basketId: 6806,
    				name: 'Article',
    				value: '4653-4877',
    				code: 'ARTICUL',
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error);
    }
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
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to add ||
|| `200240400004` | Basket item id is absent
Basket item (position) identifier is not specified ||
|| `200240400005` | Basket item id is bad
Invalid basket item (position) identifier — for example, the string ‘seventy’ ||
|| `200240400001`, `200240400002` | Basket item (position) not found ||
|| `100` | Parameter `fields` is not specified or empty ||
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
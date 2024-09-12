# Change the Property of a Basket Item sale.basketproperties.update

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
[`object`](../../data-types.md) | Values of the fields being modified (detailed description provided [below](#parametr-fields)) for the basket item (position) property:

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

- JS

    ```js
    BX24.callMethod(
        "sale.basketproperties.update",
        {
            id: 17,
            fields: {
                name: 'Article',
                value: '123-456-789',
                code: 'ARTICUL',		}
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

- PHP

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
[`sale_basket_item_property`](../data-types.md#sale_basket_item_property) | Object containing the data of the modified basket item (position) property ||
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
|| `20004030001` | Insufficient permissions to modify ||
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
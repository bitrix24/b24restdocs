# Delete Basket Property sale.basketproperties.delete

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: store administrator

This method removes a property for an item in the basket of an order.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_basket_item_property.id`](../data-types.md) | Identifier of the basket item property.

This can be obtained using the [`sale.basketproperties.list`](sale-basket-properties-list.md) method ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketproperties.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketproperties.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.basketproperties.delete",
        {
            id: 17
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.basketproperties.delete',
        [
            'id' => 17
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
    "result": true,
    "time": {
        "start": 1715855099.692296,
        "finish": 1715855106.851055,
        "duration": 7.158758878707886,
        "processing": 6.254581928253174,
        "date_start": "2024-05-16T12:24:59+02:00",
        "date_finish": "2024-05-16T12:25:06+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the delete operation ||
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
|| `200140400001` | basket item does not exist

Basket item not found   ||
|| `200040300010` | Insufficient permissions to delete ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-basket-properties-add.md)
- [{#T}](./sale-basket-properties-update.md)
- [{#T}](./sale-basket-properties-get.md)
- [{#T}](./sale-basket-properties-update.md)
- [{#T}](./sale-basket-properties-get-fields.md)
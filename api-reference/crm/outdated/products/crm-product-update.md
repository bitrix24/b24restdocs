# Update Product crm.product.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates an existing product.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Product identifier ||
|| **fields**
[`array`](../../../data-types.md) | [Set of fields](./crm-product-add.md) - an array in the format `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values returned by the method [crm.product.fields](./crm-product-fields.md). 
It is mandatory to specify `CURRENCY_ID` to set the price. 

To find out the required format of the fields, execute the method [crm.product.fields](./crm-product-property-fields.md) and check the format of the returned values for these fields.

To delete a file, the `valueId` should specify the identifier of the property value, not the file identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

### Example 1

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_product_id","fields":{"CURRENCY_ID":"USD","PRICE":5000}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_product_id","fields":{"CURRENCY_ID":"USD","PRICE":5000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.product.update
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.product.update",
        {
            id: id,
            fields:
            {
                "CURRENCY_ID": "USD",
                "PRICE": 5000
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());                        
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $id = 'your_product_id'; // Replace 'your_product_id' with the actual product ID

    $result = CRest::call(
        'crm.product.update',
        [
            'id' => $id,
            'fields' => [
                'CURRENCY_ID' => 'USD',
                'PRICE' => 5000
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Example 2

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.product.update",
        {
            id: 4611,
            fields:
            {
                "PROPERTY_186": [
                    {
                        "valueId": 0,
                        "fileData": ["1.jpg", "/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAIBAQIBAQICAgICAgICAwUDAwMDAwYEBAMFBwYH"+"BwcGBwcICQsJCAgKCAcHCg0KCgsMDAwMBwkODw0MDgsMDAz/2wBDAQICAgMDAwYDAwYMCAcIDAwMDAwMDAwMDAwMDAwMD"+"AwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAz/wAARCAARABEDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAA"+"AAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fA"+"kM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWW"+"l5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBA"+"QEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRob"+"HBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYa"+"HiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oA"+"DAMBAAIRAxEAPwDqvg78Hf8Ahawjt7eO+n1Ce5NvDDbso3YVWycg4xkkkkAAZOACa0vjF+z3J8ILO6j1CHULXULdI5Fjl"+"kR0dWYDIKjDDkjIPUEdQRR+z38YofhBcQ6hHdR2+oWt20sayQtIrqyBCDgdCNw4IPPBBwa2P2hv2ho/jdbXV1dXVu140U"+"UEMMFu8caIrhsDcM9SzZYk5PpgD+OcViuKVxTGlSi/qN1d2le/MtFpbl5d366q2vDk2TeGUvDJ4jEPBfXvqVaXvVoLEfW"+"FCfIlDnvzX5bLlu38k/F6KKK/Xj+EQooooAKKKKAP/9k="]
                    },
                    {
                        "valueId": 124,
                        "value": {"remove": "Y"}
                    }
                ]
            }
        },
        function (result) {
            if (result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.product.update',
        [
            'id' => 4611,
            'fields' => [
                'PROPERTY_186' => [
                    [
                        'valueId' => 0,
                        'fileData' => ['1.jpg', '/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAIBAQIBAQICAgICAgICAwUDAwMDAwYEBAMFBwYH'.'BwcGBwcICQsJCAgKCAcHCg0KCgsMDAwMBwkODw0MDgsMDAz/2wBDAQICAgMDAwYDAwYMCAcIDAwMDAwMDAwMDAwMDAwMD'.'AwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAz/wAARCAARABEDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAA'.'AAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fA'.'kM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWW'.'l5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBA'.'QEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRob'.'HBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYa'.'HiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oA'.'DAMBAAIRAxEAPwDqvg78Hf8Ahawjt7eO+n1Ce5NvDDbso3YVWycg4xkkkkAAZOACa0vjF+z3J8ILO6j1CHULXULdI5Fjl'.'kR0dWYDIKjDDkjIPUEdQRR+z38YofhBcQ6hHdR2+oWt20sayQtIrqyBCDgdCNw4IPPBBwa2P2hv2ho/jdbXV1dXVu140U'.'UEMMFu8caIrhsDcM9SzZYk5PpgD+OcViuKVxTGlSi/qN1d2le/MtFpbl5d366q2vDk2TeGUvDJ4jEPBfXvqVaXvVoLEfW'.'FCfIlDnvzX5bLlu38k/F6KKK/Xj+EQooooAKKKKAP/9k=']
                    ],
                    [
                        'valueId' => 124,
                        'value' => ['remove' => 'Y']
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}
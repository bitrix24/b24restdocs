# Get Fields of Trade Platforms sale.tradePlatform.getFields

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: any user with the "View product catalog" access permission

The method returns the fields of trade platforms.

No parameters.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.tradePlatform.getFields
    ```

- cURL (OAuth)

    ```
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.tradePlatform.getFields
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.tradePlatform.getFields',
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.tradePlatform.getFields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

The method returns an object containing the key `tradePlatform` — a list of fields of trade platforms.

HTTP status: **200**

```json
{ 
    "result": { 
        "tradePlatform": { 
            "catalogSectionTabClassName": { 
                "isImmutable": false,
                "isReadOnly": false, 
                "isRequired": false, 
                "type": "string" 
            }, 
            "class": { 
                "isImmutable": false, 
                "isReadOnly": false, 
                "isRequired": false, 
                "type": "string" 
            } 
            // ... other fields 
        } 
    },
    "time": {  
        "start": 1712135957.057659,  
        "finish": 1712135957.407821,  
        "duration": 0.3501620292663574,  
        "processing": 0.011919021606445312,  
        "date_start": "2024-04-03T11:19:17+02:00",  
        "date_finish": "2024-04-03T11:19:17+02:00",  
        "operating_reset_at": 1705765533,  
        "operating": 3.3076241016387939  
    }  
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **tradePlatform**
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. Where `field` is the identifier of the object [sale_order_trade_platform](../data-types.md), and `value` is an object of type [rest_field_description](../data-types.md#rest_field_description) ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to execute the method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-trade-platform-list.md)
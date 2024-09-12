# Get a list of trade platforms sale.tradePlatform.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: any user with the "View product catalog" access permission

This method retrieves a list of trade platforms.

## Method Parameters

#|
|| **Name**
`Type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the [sale_order_trade_platform](../data-types.md#sale_order_trade_platform) object)
  ||
|| **filter**
[`object`](../../data-types.md) | A list of fields for filtering. When multiple fields are specified, AND logic is applied.

The fields correspond to the fields of the [sale_order_trade_platform](../data-types.md#sale_order_trade_platform) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol should not be included in the filter value. The search looks for the substring in any position of the string
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol should not be included in the filter value. The search is performed from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol should be included in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol should be included in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
 ||
|| **order**
[`object`](../../data-types.md) | Sorting parameters. Format: `{field: direction (ASC, DESC)}`. 

The fields correspond to the fields of the [sale_order_trade_platform](../data-types.md#sale_order_trade_platform) object. ||
|| **start**
[`int`](../../data-types.md) | This parameter is used for pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","code"],"filter":{"%code":"smart"},"order":{"code":"asc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.tradePlatform.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","code"],"filter":{"%code":"smart"},"order":{"code":"asc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.tradePlatform.list
    ```

- JS

    ```js
    BX24.callMethod( 
        "sale.tradePlatform.list", 
        {
            select: ['id', 'code'],
            filter: {'%code': 'smart'},
            order: {'code': 'asc'},
            start: 0,
        }, 
        function(result) 
        { 
            if(result.error()) 
            {
                console.error(result);
            }
            else
            {
                console.dir(result);
            } 
        } 
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.tradePlatform.list',
        [
            'select' => ['id', 'code'],
            'filter' => ['%code' => 'smart'],
            'order' => ['code' => 'asc'],
            'start' => 0,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

The method returns an object containing the key `tradePlatforms` — a list of trade platforms.

HTTP status: **200**

```json
{
    "result": {
        "tradePlatforms": [
            {
                "code": "smart_invoice",
                "id": 2
            }
        ]
    },
    "total": 1,
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
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **tradePlatforms**
[`sale_order_trade_platform[]`](../data-types.md) | An array of objects containing information about trade platforms ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
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
- [{#T}](./sale-trade-platform-get-fields.md)
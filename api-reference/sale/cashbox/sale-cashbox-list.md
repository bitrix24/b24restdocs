# Get a List of Configured Cash Registers sale.cashbox.list

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to modify settings")

This method returns a list of configured cash registers.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../../data-types.md) | An array of fields to select (see fields of the [sale_cashbox](../data-types.md#sale_cashbox) object) ||
|| **FILTER**
[`object`](../../data-types.md) | An object for filtering selected cash registers in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_cashbox](../data-types.md#sale_cashbox) object.

When specifying multiple fields, the logic `AND` is used.

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol needs to be included in the value. Examples:
    - `"milk%"` — searches for values starting with "milk"
    - `"%milk"` — searches for values ending with "milk"
    - `"%milk%"` — searches for values where "milk" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be included in the value. Examples:
    - `"milk%"` — searches for values not starting with "milk"
    - `"%milk"` — searches for values not ending with "milk"
    - `"%milk%"` — searches for values where the substring "milk" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **ORDER**
[`object`](../../data-types.md) | An object for sorting selected records in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_cashbox](../data-types.md#sale_cashbox) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|#

## Code Examples

{% include [Footnote on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","NAME"],"FILTER":{"=NAME":"My Rest Cash Register",">ID":9},"ORDER":{"ID":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.cashbox.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","NAME"],"FILTER":{"=NAME":"My Rest Cash Register",">ID":9},"ORDER":{"ID":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.cashbox.list
    ```

- JS

    ```js
    BX24.callMethod( 
        "sale.cashbox.list", 
        { 
            "SELECT": ["ID", "NAME"], 
            "FILTER": {"=NAME": "My Rest Cash Register", ">ID": 9},
            "ORDER": {"ID": "DESC"},
        }, 
        function(result) 
        { 
            if(result.error()) 
                console.error(result.error()); 
            else 
                console.dir(result.data()); 
        } 
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.cashbox.list',
        [
            'SELECT' => ['ID', 'NAME'],
            'FILTER' => ['=NAME' => 'My Rest Cash Register', '>ID' => 9],
            'ORDER' => ['ID' => 'DESC']
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
    "result": [
        {
            "ID": "1",
            "NAME": "REST Cash Register",
            "OFD": "bx_firstofd",
            "EMAIL": "admin@example.com",
            "DATE_CREATE": {},
            "DATE_LAST_CHECK": null,
            "NUMBER_KKM": "",
            "KKM_ID": null,
            "ACTIVE": "Y",
            "SORT": "100",
            "USE_OFFLINE": "N",
            "ENABLED": "Y"
        }
    ],
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
[`sale_cashbox[]`](../data-types.md#sale_cashbox) | An array of cash registers registered in the system  ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "BAD_FIELD not allowed for select"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient permissions to retrieve the list of cash registers | 403 ||
|| ‘’ (empty error code) | An incorrect field was specified for selection, filtering, or sorting. More detailed information about the error can be found in `error_description` | 400 ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-cashbox-handler-add.md)
- [{#T}](./sale-cashbox-handler-update.md)
- [{#T}](./sale-cashbox-handler-list.md)
- [{#T}](./sale-cashbox-handler-delete.md)
- [{#T}](./sale-cashbox-add.md)
- [{#T}](./sale-cashbox-update.md)
- [{#T}](./sale-cashbox-delete.md)
- [{#T}](./sale-cashbox-check-apply.md)
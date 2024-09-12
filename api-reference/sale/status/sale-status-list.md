# Get the List of Statuses sale.status.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of statuses.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to select (see fields of the [sale_status](../data-types.md) object).

If the array is not provided or is empty, all available status fields will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected property groups in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_status](../data-types.md) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring at any position in the string.
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be at any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search is performed from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present at any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
 ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected statuses in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_status](../data-types.md) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number.
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
    -d '{"select":["id","type","notify","color","sort","xmlId"],"filter":{"id":"N"},"order":{"type":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.status.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","type","notify","color","sort","xmlId"],"filter":{"id":"N"},"order":{"type":"asc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.status.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.status.list", {
            "select": [
                "id",
                "type",
                "notify",
                "color",
                "sort",
                "xmlId",
            ],
            "filter": {
                "id": "N",
            },
            "order": {
                "type": "asc",
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call('sale.status.list', [
        'select' => [
            'id',
            'type',
            'notify',
            'color',
            'sort',
            'xmlId',
        ],
        'filter' => [
            'id' => 'N',
        ],
        'order' => [
            'type' => 'asc',
        ]
    ]);

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result":{
        "statuses":[
            {
                "color":"#BEEDF1",
                "id":"N",
                "notify":"Y",
                "sort":10,
                "type":"O",
                "xmlId":null
            }
        ]
    },
    "total":1,
    "time":{
        "start":1712655587.593758,
        "finish":1712655587.816158,
        "duration":0.22239995002746582,
        "processing":0.016273975372314453,
        "date_start":"2024-04-09T12:39:47+03:00",
        "date_finish":"2024-04-09T12:39:47+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **status**
[`sale_status[]`](../data-types.md) | An array of objects containing information about the selected statuses ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
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
|| `200040300010` | Insufficient permissions to read statuses ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-status-add.md)
- [{#T}](./sale-status-update.md)
- [{#T}](./sale-status-get.md)
- [{#T}](./sale-status-delete.md)
- [{#T}](./sale-status-get-fields.md)
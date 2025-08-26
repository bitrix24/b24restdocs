# Get the list of statuses sale.status.list

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

If the array is not provided or an empty array is passed, all available status fields will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering selected property groups in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_status](../data-types.md) object.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
 ||
|| **order**
[`object`](../../data-types.md) | An object for sorting selected statuses in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

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
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    const method = "sale.status.list";
    const parameters = {
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
    };
    
    try {
        const response = await $b24.callListMethod(method, parameters);
        const items = response.getData() || [];
        for (const entity of items) {
            console.log('Entity:', entity);
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative fetching using a generator, allowing data to be processed in chunks and efficiently using memory.
    
    try {
        const generator = $b24.fetchListMethod(method, parameters, 'ID');
        for await (const page of generator) {
            for (const entity of page) {
                console.log('Entity:', entity);
            }
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
        const response = await $b24.callMethod(method, parameters, 0);
        const result = response.getData().result || [];
        for (const entity of result) {
            console.log('Entity:', entity);
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.status.list',
                [
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
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching sale status list: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

HTTP status: **200**

```json
{
    "result": {
        "statuses": [
            {
                "color": "#BEEDF1",
                "id": "N",
                "notify": "Y",
                "sort": 10,
                "type": "O",
                "xmlId": null
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1712655587.593758,
        "finish": 1712655587.816158,
        "duration": 0.22239995002746582,
        "processing": 0.016273975372314453,
        "date_start": "2024-04-09T12:39:47+02:00",
        "date_finish": "2024-04-09T12:39:47+02:00"
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
[`sale_status[]`](../data-types.md) | An array of objects with information about the selected statuses ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "error"
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
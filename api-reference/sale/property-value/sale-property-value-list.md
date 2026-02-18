# Get a list of property values sale.propertyvalue.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of order property value options.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to be selected (see fields of the [sale_order_property_value](../data-types.md) object).

If the array is not provided or an empty array is passed, all available property value fields will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering selected property values in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_property_value](../data-types.md) object.

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for a substring at any position in the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be at any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present at any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | An object for sorting selected property values in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_property_value](../data-types.md) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static: 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["code","id","name","orderId","orderPropsId","orderPropsXmlId","value"],"filter":{"=code":"FIO","%value":"Boris",">orderId":1600},"order":{"orderId":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvalue.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["code","id","name","orderId","orderPropsId","orderPropsXmlId","value"],"filter":{"=code":"FIO","%value":"Boris",">orderId":1600},"order":{"orderId":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvalue.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    const parameters = {
        "select": [
            "code",
            "id",
            "name",
            "orderId",
            "orderPropsId",
            "orderPropsXmlId",
            "value",
        ],
        "filter": {
            "=code": "FIO",
            "%value": "Boris",
            ">orderId": 1600,
        },
        "order": {
            "orderId": "desc",
        },
    };
    
    try {
        const response = await $b24.callListMethod(
            'sale.propertyvalue.list',
            parameters,
            (progress) => { console.log('Progress:', progress) }
        );
        const items = response.getData() || [];
        for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
        const generator = $b24.fetchListMethod('sale.propertyvalue.list', parameters, 'ID');
        for await (const page of generator) {
            for (const entity of page) { console.log('Entity:', entity); }
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
        const response = await $b24.callMethod('sale.propertyvalue.list', parameters, 0);
        const result = response.getData().result || [];
        for (const entity of result) { console.log('Entity:', entity); }
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
                'sale.propertyvalue.list',
                [
                    'select' => [
                        'code',
                        'id',
                        'name',
                        'orderId',
                        'orderPropsId',
                        'orderPropsXmlId',
                        'value',
                    ],
                    'filter' => [
                        '=code'    => 'FIO',
                        '%value'   => 'Boris',
                        '>orderId' => 1600,
                    ],
                    'order' => [
                        'orderId' => 'desc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching sale property values: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.propertyvalue.list", {
            "select": [
                "code",
                "id",
                "name",
                "orderId",
                "orderPropsId",
                "orderPropsXmlId",
                "value",
            ],
            "filter": {
                "=code": "FIO",
                "%value": "Boris",
                ">orderId": 1600,
            },
            "order": {
                "orderId": "desc",
            },
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

    $result = CRest::call(
        'sale.propertyvalue.list',
        [
            'select' => [
                'code',
                'id',
                'name',
                'orderId',
                'orderPropsId',
                'orderPropsXmlId',
                'value',
            ],
            'filter' => [
                '=code' => 'FIO',
                '%value' => 'Boris',
                '>orderId' => 1600,
            ],
            'order' => [
                'orderId' => 'desc',
            ],
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
    "result": {
        "propertyValues": [
            {
                "code": "FIO",
                "id": 10774,
                "name": "First Last Name",
                "orderId": 1650,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10763,
                "name": "First Last Name",
                "orderId": 1649,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10723,
                "name": "First Last Name",
                "orderId": 1641,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10718,
                "name": "First Last Name",
                "orderId": 1640,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10713,
                "name": "First Last Name",
                "orderId": 1639,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10708,
                "name": "First Last Name",
                "orderId": 1638,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10687,
                "name": "First Last Name",
                "orderId": 1634,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            },
            {
                "code": "FIO",
                "id": 10517,
                "name": "First Last Name",
                "orderId": 1603,
                "orderPropsId": 20,
                "orderPropsXmlId": null,
                "value": "Sokolov Boris Viktorovich"
            }
        ]
    },
    "total": 8,
    "time": {
        "start": 1712061753.171393,
        "finish": 1712061753.431631,
        "duration": 0.2602381706237793,
        "processing": 0.021820783615112305,
        "date_start": "2024-04-02T15:42:33+02:00",
        "date_finish": "2024-04-02T15:42:33+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyValues**
[`sale_order_property_value[]`](../data-types.md) | An array of objects with information about the selected property values ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `200040300010` | Insufficient permissions to read property values ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-value-modify.md)
- [{#T}](./sale-property-value-get.md)
- [{#T}](./sale-property-value-delete.md)
- [{#T}](./sale-property-value-get-fields.md)


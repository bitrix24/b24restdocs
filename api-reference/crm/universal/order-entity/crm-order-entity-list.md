# Get a list of order bindings to CRM entities crm.orderentity.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: online store manager

The method returns a list of order bindings to CRM entities.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array of fields to select (see fields of the [crm_orderentity](../../data-types.md#crm_orderentity) object).

If the array is not provided or an empty array is passed, all available fields of the order binding to the CRM entity will be selected.
||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering selected records in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field_N` correspond to the fields of the [crm_orderentity](../../data-types.md#crm_orderentity) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character should not be passed in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` character must be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character should not be passed in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` character must be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal 
||
|| **order**
[`object`](../../../data-types.md) | An object for sorting records in the format `{"field_1": "order_1", ... "field_N": "order_N"}`, where `field_N` is the identifier of the [crm_orderentity](../../data-types.md#crm_orderentity) field.

Possible values for `order_N`:
- `asc` — in ascending order
- `desc` — in descending order

If the object is not provided or an empty object is passed, sorting will be in ascending order of the [crm_orderentity.OWNER_ID](../../data-types.md#crm_orderentity) field.
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number.

If you specify the value `-1`, all records that meet the filter conditions will be selected.
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Get the IDs of orders linked to three deals:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["orderId","ownerId"],"filter":{"=ownerTypeId":2,"@ownerId":[6938,6937,6933]},"order":{"orderId":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.orderentity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["orderId","ownerId"],"filter":{"=ownerTypeId":2,"@ownerId":[6938,6937,6933]},"order":{"orderId":"asc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.orderentity.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.orderentity.list',
        {
          select: [
            'orderId',
            'ownerId',
          ],
          filter: {
            '=ownerTypeId': 2,
            '@ownerId': [6938, 6937, 6933],
          },
          order: {
            orderId: 'asc'
          },
        },
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.orderentity.list', {
        select: [
          'orderId',
          'ownerId',
        ],
        filter: {
          '=ownerTypeId': 2,
          '@ownerId': [6938,6937,6933],
        },
        order: {
          orderId: 'asc'
        },
      }, 'orderId');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.orderentity.list', {
        select: [
          'orderId',
          'ownerId',
        ],
        filter: {
          '=ownerTypeId': 2,
          '@ownerId': [6938, 6937, 6933],
        },
        order: {
          orderId: 'asc'
        },
      }, 0);
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
                'crm.orderentity.list',
                [
                    'select' => [
                        'orderId',
                        'ownerId',
                    ],
                    'filter' => [
                        '=ownerTypeId' => 2,
                        '@ownerId'    => [6938, 6937, 6933],
                    ],
                    'order'  => [
                        'orderId' => 'asc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching order entities: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.orderentity.list",
        {
            select: [
                'orderId',
                'ownerId',
            ],
            filter: {
                '=ownerTypeId': 2,
                '@ownerId': [6938, 6937, 6933],
            },
            order: {
                orderId: 'asc'
            },
        },)
        .then(
            function(result)
            {
                if (result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    console.log(result.data);
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
        'crm.orderentity.list',
        [
            'select' => [
                'orderId',
                'ownerId',
            ],
            'filter' => [
                '=ownerTypeId' => 2,
                '@ownerId' => [6938, 6937, 6933],
            ],
            'order' => [
                'orderId' => 'asc'
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
        "orderEntity": [
            {
                "orderId": 5125,
                "ownerId": 6933
            },
            {
                "orderId": 5127,
                "ownerId": 6937
            },
            {
                "orderId": 5128,
                "ownerId": 6938
            }
        ]
    },
    "total": 3,
    "time": {
        "start": 1719315806.858516,
        "finish": 1719315807.569731,
        "duration": 0.7112150192260742,
        "processing": 0.039324045181274414,
        "date_start": "2024-06-25T13:43:26+02:00",
        "date_finish": "2024-06-25T13:43:27+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The root element of the response ||
|| **orderEntity**
[`crm_orderentity[]`](../../data-types.md#crm_orderentity) | An array of objects with information about the selected orders ||
|| **total**
[`integer`](../../../data-types.md) | The total number of selected records ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "200040300010",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Description** ||
|| `200040300010` | `Access Denied` 
Insufficient access permissions
||
|| `200540400002` | `module sale does not exist` 
The `Online Store` (sale) module is missing
||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-order-entity-add.md)
- [{#T}](./crm-order-entity-delete-by-filter.md)
- [{#T}](./crm-order-entity-get-fields.md)
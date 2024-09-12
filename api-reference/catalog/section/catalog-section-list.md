# Get a List of Sections from the Trade Catalog catalog.section.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `catalog.section.list` allows you to retrieve a list of sections from the trade catalog.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the [catalog_section](../data-types.md#catalog_section) object).

If not provided or an empty array is passed, all available fields of the catalog section will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected sections of the catalog in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_section](../data-types.md#catalog_section) object.

**The `iblockId` field is required**.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` character should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character should be included in the value. Examples:
    - "milk%" — searching for values starting with "milk"
    - "%milk" — searching for values ending with "milk"
    - "%milk%" — searching for values where "milk" can be in any position

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` character should not be included in the filter value. The search goes from both sides.

- `!%=` — NOT LIKE (see description above)

- `=` — equal, exact match (used by default)
- `!=` - not equal ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected sections of the catalog in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_section](../data-types.md#catalog_section) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "select": ["id", "name", "sort"],
        "filter": {
            "iblockId": 14,
            "<=sort": 100
        },
        "order": {
            "sort": "DESC"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.section.list
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "select": ["id", "name", "sort"],
        "filter": {
            "iblockId": 14,
            "<=sort": 100
        },
        "order": {
            "sort": "DESC"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/catalog.section.list
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.section.list', 
        {
            select: ["id", "name", "sort"],
            filter: {
                'iblockId': 14,
                '<=sort': 100
            },
            order: {'sort': 'DESC'}
        }, 
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
        'catalog.section.list',
        [
            'select' => ["id", "name", "sort"],
            'filter' => [
                'iblockId' => 14,
                '<=sort' => 100
            ],
            'order' => [
                'sort' => 'DESC'
            ]
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
    "sections": [
        {
            "id": 13,
            "name": "Products",
            "sort": 500
        },
        {
            "id": 14,
            "name": "Services",
            "sort": 500
        },
        {
            "id": 22,
            "name": "Footwear",
            "sort": 50
        }
    ],
    "total": 3,
    "time": {
        "start": 1712326352.63409,
        "finish": 1712326352.8319,
        "duration": 0.197818040847778,
        "processing": 0.00833678245544434,
        "date_start": "2024-04-05T16:12:32+02:00",
        "date_finish": "2024-04-05T16:12:32+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **section**
[`catalog_section[]`](../data-types.md#catalog_section) | An array of objects containing information about the selected sections of the catalog ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300030,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300030` | Insufficient permissions to read the catalog section ||
|| `0` | Required fields of the `filter` structure are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-section-add.md)
- [{#T}](./catalog-section-update.md)
- [{#T}](./catalog-section-get.md)
- [{#T}](./catalog-section-delete.md)
- [{#T}](./catalog-section-get-fields.md)
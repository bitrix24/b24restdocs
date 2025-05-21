# Get the list of datasets biconnector.dataset.list

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Analyst's workspace" section

The method `biconnector.dataset.list` returns a list of datasets based on a filter. It is an implementation of the listing method for datasets.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | A list of fields that must be populated in the datasets in the selection. By default, all fields are included. 
The `fields` parameter is not supported and will be ignored. ||
|| **filter**
[`object`](../../data-types.md) | Filter for selecting datasets. Example format:

```json
{
    "field_1": "value_1",
    "field_2": "value_2"
}
```

You can add a prefix to the keys `field_n` to specify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character must be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The list of available fields for filtering can be obtained using the method [biconnector.dataset.fields](./biconnector-dataset-fields.md).

The `fields` parameter is not supported and will be ignored. ||
|| **order**
[`object`](../../data-types.md) | Sorting parameters. Example format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the field by which the dataset selection will be sorted
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort
||
|| **page**
[`integer`](../../data-types.md) | Controls pagination. The page size for results is 50 records. To navigate through results, pass the page number. ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Get a list of sources where:
- the name starts with `Sales`
- the description is not empty
- the source ID is `2` or `4`

To illustrate, select only the necessary fields:
- ID `id`
- Name `name`
- Description `description`

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'biconnector.dataset.list',
        {
            select: ["id", "name", "description"],
            filter: {
                '%=name': "Sales%",
                '!description': "",
                "@sourceId": [2, 4]
            },
            order: {
                dateCreate: "DESC"
            }
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "select": ["id", "name", "description"],
        "filter": {
            "%=name": "Sales%",
            "!description": "",
            "@sourceId": [2, 4]
        },
        "order": {
            "dateCreate": "DESC"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.dataset.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "select": ["id", "name", "description"],
        "filter": {
            "%=name": "Sales%",
            "!description": "",
            "@sourceId": [2, 4]
        },
        "order": {
            "dateCreate": "DESC"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/biconnector.dataset.list
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.dataset.list',
        [
            'select' => ["id", "name", "description"],
            'filter' => ['%=name' => "Sales%", '!description' => "", '@sourceId' => [2, 4]],
            'order' => ['dateCreate' => "DESC"]
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
            "id": "9",
            "name": "sales_data_main",
            "description": "Monthly sales report"
        },
        {
            "id": "6",
            "name": "sales_data_first_branch",
            "description": "Monthly sales report for first branch"
        },
        {
            "id": "5",
            "name": "sales_data_second_branch",
            "description": "Monthly sales report for second branch"
        }
    ],
    "time": {
        "start": 1743061675.963969,
        "finish": 1743061676.064591,
        "duration": 0.10062193870544434,
        "processing": 0.011152029037475586,
        "date_start": "2025-03-27T07:47:55+00:00",
        "date_finish": "2025-03-27T07:47:56+00:00"
    }
}
```

### Returned Data

#|
|| **result**
[`object`](../../data-types.md) | The root element of the response. Contains an array of objects with information about the dataset fields. 

It should be noted that the structure of the fields may change due to the `select` parameter. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **200**

```json
{
    "error": "VALIDATION_SELECT_TYPE",
    "error_description": "Parameter \"select\" must be array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `VALIDATION_SELECT_TYPE` | `Parameter "select" must be array.` | The `select` parameter is not an object. ||
|| `VALIDATION_FILTER_TYPE` | `Parameter "filter" must be array.` | The `filter` parameter is not an object. ||
|| `VALIDATION_ORDER_TYPE` | `Parameter "order" must be array.` | The `order` parameter is not an object. ||
|| `VALIDATION_FIELD_NOT_ALLOWED_IN_SELECT` | `Field "#TITLE#" is not allowed in the "select".` | These fields are not allowed in the selection. ||
|| `VALIDATION_FIELD_NOT_ALLOWED_IN_FILTER` | `Field "#TITLE#" is not allowed in the "filter".` | These fields are not allowed in the filter. ||
|| `VALIDATION_FIELD_NOT_ALLOWED_IN_ORDER` | `Field "#TITLE#" is not allowed in the "order".` | These fields are not allowed for sorting. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-dataset-add.md)
- [{#T}](./biconnector-dataset-update.md)
- [{#T}](./biconnector-dataset-fields-update.md)
- [{#T}](./biconnector-dataset-get.md)
- [{#T}](./biconnector-dataset-fields.md)
- [{#T}](./biconnector-dataset-delete.md)
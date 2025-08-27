# Get the list of sources biconnector.source.list

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Analyst Workspace" section

The method `biconnector.source.list` returns a list of sources based on a filter. It is an implementation of the list method for sources.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | List of fields that must be filled in the sources in the selection. By default, all fields are taken ||
|| **filter**
[`object`](../../data-types.md) | Filter for selecting sources. Example format:

```json
{
    "field_1": "value_1",
    "field_2": "value_2"
}
```

You can add a prefix to the keys `field_n` to clarify the filter's operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for a substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The list of available fields for filtering can be found using the method [biconnector.source.fields](./biconnector-source-fields.md).

The filter does not support the `settings` field; it will be ignored
||
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

- `field_n` — the name of the field by which the sources will be sorted
- `value_n` — a `string` type value, equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort
||
|| **page**
[`integer`](../../data-types.md) | Controls pagination. The page size of results is 50 records. To navigate through results, pass the page number  ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Get the list of sources where:
- the name starts with `Sql`
- the description is not empty
- the connector ID is equal to `2` or `4`

Display only the necessary fields:
- ID `id`
- Title `title`
- Active `active`
- Creation date `dateCreate`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","active","dateCreate"],"filter":{"%=title":"Sql%","!description":"","@connectorId":[2,4]},"order":{"dateCreate":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.source.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","active","dateCreate"],"filter":{"%=title":"Sql%","!description":"","@connectorId":[2,4]},"order":{"dateCreate":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/biconnector.source.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to get the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    const selectFields = [
        "id",
        "title",
        "active",
        "dateCreate"
    ];
    
    try {
        const response = await $b24.callListMethod(
            'biconnector.source.list',
            {
                select: selectFields,
                filter: {
                    '%=title': "Sql%",
                    '!description': "",
                    "@connectorId": [2, 4]
                },
                order: {
                    dateCreate: "DESC"
                }
            },
            (progress) => { console.log('Progress:', progress) }
        );
        const items = response.getData() || [];
        for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    const selectFields = [
        "id",
        "title",
        "active",
        "dateCreate"
    ];
    
    try {
        const generator = $b24.fetchListMethod('biconnector.source.list', {
            select: selectFields,
            filter: {
                '%=title': "Sql%",
                '!description': "",
                "@connectorId": [2, 4]
            },
            order: {
                dateCreate: "DESC"
            }
        }, 'ID');
        for await (const page of generator) {
            for (const entity of page) { console.log('Entity:', entity); }
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    const selectFields = [
        "id",
        "title",
        "active",
        "dateCreate"
    ];
    
    try {
        const response = await $b24.callMethod('biconnector.source.list', {
            select: selectFields,
            filter: {
                '%=title': "Sql%",
                '!description': "",
                "@connectorId": [2, 4]
            },
            order: {
                dateCreate: "DESC"
            }
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
                'biconnector.source.list',
                [
                    'select' => [
                        "id",
                        "title",
                        "active",
                        "dateCreate"
                    ],
                    'filter' => [
                        '%=title'      => "Sql%",
                        '!description' => "",
                        "@connectorId" => [2, 4]
                    ],
                    'order'  => [
                        'dateCreate' => "DESC"
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching source list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.source.list',
        {
            select: [
                "id",
                "title",
                "active",
                "dateCreate"
            ],
            filter: {
                '%=title': "Sql%",
                '!description': "",
                "@connectorId": [2, 4]
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.source.list',
        [
            'select' => [
                "id",
                "title",
                "active",
                "dateCreate"
            ],
            'filter' => [
                '%=title' => "Sql%",
                '!description' => "",
                '@connectorId' => [2, 4]
            ],
            'order' => [
                'dateCreate' => "DESC"
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
    "result": [
        {
            "id": "11",
            "title": "Sql_host",
            "active": true,
            "dateCreate": "2025-03-24 07:25:59"
        },
        {
            "id": "10",
            "title": "Sql_partner",
            "active": false,
            "dateCreate": "2025-03-21 12:22:32"
        }
    ],
    "time": {
        "start": 1742804947.923552,
        "finish": 1742804947.995446,
        "duration": 0.07189393043518066,
        "processing": 0.0017020702362060547,
        "date_start": "2025-03-24T08:29:07+00:00",
        "date_finish": "2025-03-24T08:29:07+00:00"
    }
}
```

### Returned Data

#|
|| **result**
[`object`](../../data-types.md) | The root element of the response. Contains an array of objects with information about the fields of the sources. 

It should be noted that the structure of the fields may change due to the `select` parameter ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **200**

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
|| `VALIDATION_SELECT_TYPE` | Parameter "select" must be array. | The `select` parameter is not an object ||
|| `VALIDATION_FILTER_TYPE` | Parameter "filter" must be array. | The `filter` parameter is not an object ||
|| `VALIDATION_ORDER_TYPE` | Parameter "order" must be array. | The `order` parameter is not an object ||
|| `VALIDATION_FIELD_NOT_ALLOWED_IN_SELECT` | Field "#TITLE#" is not allowed in the "select". | These fields are not allowed in the selection ||
|| `VALIDATION_FIELD_NOT_ALLOWED_IN_FILTER` | Field "#TITLE#" is not allowed in the "filter". | These fields are not allowed in the filter ||
|| `VALIDATION_FIELD_NOT_ALLOWED_IN_ORDER` | Field "#TITLE#" is not allowed in the "order". | These fields are not allowed for sorting ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-source-update.md)
- [{#T}](./biconnector-source-get.md)
- [{#T}](./biconnector-source-add.md)
- [{#T}](./biconnector-source-delete.md)
- [{#T}](./biconnector-source-fields.md)
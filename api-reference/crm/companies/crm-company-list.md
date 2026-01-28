# Get a List of Companies by Filter crm.company.list

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Read" access permission for companies

{% note warning "Method development has been halted" %}

The method `crm.company.list` continues to function, but there is a more current equivalent [crm.item.list](../universal/crm-item-list.md).

{% endnote %}

The method `crm.company.list` returns a list of companies based on a filter. It is an implementation of the [list method](../../../settings/how-to-call-rest-api/list-methods-pecularities.md) for companies.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | List of fields to limit the selection.

You can use masks for selection:
- `'*'` — to select all fields, excluding custom and multiple fields,
- `'UF_*'` — to select all custom fields excluding multiple ones.

There are no masks for selecting multiple fields. To select multiple fields, specify the required ones in the selection list — `PHONE`, `EMAIL`, and so on.

You can find the list of available fields for selection using the method [crm.company.fields](crm-company-fields.md).

By default, all fields are returned — `'*'` + custom fields — `'UF_*'`
||
|| **filter**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the field by which the selection of elements will be filtered
- `value_n` — the filter value

You can add a prefix to the keys `field_n` to clarify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for a substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol must be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The fields `PHONE`, `EMAIL`, `WEB`, `IM` are multiple. Filters work only on exact matches for them.

The `LIKE` filter does not work with fields of type `crm_status`, `crm_company` — for example, `COMPANY_TYPE`.

You can find the list of available fields for filtering using the method [crm.company.fields](crm-company-fields.md).

The `logic` key in the filter is not supported. To use complex logic in the filter, use the method [crm.item.list](../universal/crm-item-list.md)
||
|| **order**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where:
- `field_n` — the name of the field by which the selection of companies will be sorted
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

You can find the list of available fields for sorting using the method [crm.company.fields](crm-company-fields.md)
||
|| **start**
[`integer`](../../data-types.md) | Parameter for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Retrieve companies:
- sorted by creation date,
- with selected fields: name, responsible person, phone,
- filtered by: company type `CUSTOMER` and creation date from 2025-01-01.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ORDER":{"DATE_CREATE":"ASC"},"FILTER":{"COMPANY_TYPE":"CUSTOMER",">=DATE_CREATE":"2025-01-01"},"SELECT":["TITLE","ASSIGNED_BY_ID","PHONE"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ORDER":{"DATE_CREATE":"ASC"},"FILTER":{"COMPANY_TYPE":"CUSTOMER",">=DATE_CREATE":"2025-01-01"},"SELECT":["TITLE","ASSIGNED_BY_ID","PHONE"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once.
    // Use only for small selections (< 1000 items) due to high
    // memory load.

    try {
    const response = await $b24.callListMethod(
        'crm.company.list',
        {
        order: { "DATE_CREATE": "ASC" },
        filter: { "COMPANY_TYPE": "CUSTOMER", ">=DATE_CREATE": "2025-01-01" },
        select: [ "TITLE", "ASSIGNED_BY_ID", "PHONE" ]
        },
        (progress: number) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Retrieves data in parts using an iterator.
    // Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('crm.company.list', {
        order: { "DATE_CREATE": "ASC" },
        filter: { "COMPANY_TYPE": "CUSTOMER", ">=DATE_CREATE": "2025-01-01" },
        select: [ "TITLE", "ASSIGNED_BY_ID", "PHONE" ]
    }, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // callMethod: Manual control of pagination through the start parameter.
    // Use for precise control over request batches.
    // Less efficient for large data than fetchListMethod.

    try {
    const response = await $b24.callMethod('crm.company.list', {
        order: { "DATE_CREATE": "ASC" },
        filter: { "COMPANY_TYPE": "CUSTOMER", ">=DATE_CREATE": "2025-01-01" },
        select: [ "TITLE", "ASSIGNED_BY_ID", "PHONE" ]
    }, 0);
    const result = response.getData().result || [];
    for (const entity of result) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.list',
                [
                    'order' => ['DATE_CREATE' => 'ASC'],
                    'filter' => ['COMPANY_TYPE' => 'CUSTOMER', '>=DATE_CREATE' => '2025-01-01'],
                    'select' => ['TITLE', 'ASSIGNED_BY_ID', 'PHONE'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
        if ($result->more()) {
            $result->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching company list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.list",
        {
            order: { "DATE_CREATE": "ASC" },
            filter: { "COMPANY_TYPE": "CUSTOMER", ">=DATE_CREATE": "2025-01-01" },
            select: [ "TITLE", "ASSIGNED_BY_ID", "PHONE" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.company.list',
        [
            'order' => ['DATE_CREATE' => 'ASC'],
            'filter' => ['COMPANY_TYPE' => 'CUSTOMER', '>=DATE_CREATE' => '2025-01-01'],
            'select' => ['TITLE', 'ASSIGNED_BY_ID', 'PHONE'],
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
            "TITLE": "Crossroads",
            "ASSIGNED_BY_ID": "811",
            "ID": "2919",
            "PHONE": [
                {
                    "ID": "8303",
                    "VALUE_TYPE": "WORK",
                    "VALUE": "+19998887766",
                    "TYPE_ID": "PHONE"
                }
            ]
        }
    ],
    "total": 1,
    "time": {
        "start": 1769498859,
        "finish": 1769498859.948139,
        "duration": 0.948138952255249,
        "processing": 0,
        "date_start": "2026-01-27T10:27:39+01:00",
        "date_finish": "2026-01-27T10:27:39+01:00",
        "operating_reset_at": 1769499459,
        "operating": 0.1621239185333252
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array[]`](../../data-types.md) | Array of companies matching the filter. The format of the returned data depends on the `select` parameter ||
|| **total**
[`integer`](../../data-types.md) | Total number of companies found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | `Access denied` | The user does not have "Read" access permission for companies ||
|| `-`     | `Parameter 'order' must be array` | The `order` parameter is not an array ||
|| `-`     | `Parameter 'filter' must be array` | The `filter` parameter is not an array ||
|| `-`     | `Failed to get list. General error` | An unknown error occurred ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-add.md)
- [{#T}](./crm-company-update.md)
- [{#T}](./crm-company-get.md)
- [{#T}](./crm-company-delete.md)
- [{#T}](./crm-company-fields.md)
- [{#T}](../../../tutorials/crm/how-to-get-lists/search-by-phone-and-email.md)
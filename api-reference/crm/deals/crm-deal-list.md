# Get a list of deals crm.deal.list

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "read" access permission for deals

{% note warning "Method Development Stopped" %}

The method `crm.deal.list` continues to function, but there is a more relevant alternative [crm.item.list](../universal/crm-item-list.md).

{% endnote %}

The method `crm.deal.list` returns a list of deals based on a filter. It is an implementation of the list method for deals.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | List of fields that should be populated for deals in the selection.

You can use the following masks for selection:
- `'*'` — to select all fields (excluding custom and multiple fields)
- `'UF_*'` — to select all custom fields (excluding multiple fields)

You can find the list of available fields for selection using the method [crm.deal.fields](./crm-deal-fields.md). 
The method does not support the field `CONTACT_IDS`; to get deals with a list of contacts, use the method [crm.item.list](../universal/crm-item-list.md).

By default, all fields are taken — `'*'` + Custom fields — `'UF_*'`
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
- `%` — LIKE, substring search. The `%` character in the filter value should not be passed. The search looks for a substring in any position of the string
- `=%` — LIKE, substring search. The `%` character should be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The LIKE filter does not work with fields of type `crm_status`, `crm_contact`, `crm_company` (deal type `TYPE_ID`, stage `STAGE_ID`, etc.).

You can find the list of available fields for filtering using the method [crm.deal.fields](./crm-deal-fields.md). 

The filter does not support the field `CONTACT_IDS`; to filter by contacts, use the method [crm.item.list](../universal/crm-item-list.md)
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
- `field_n` — the name of the field by which the selection of deals will be sorted
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

You can find the list of available fields for sorting using the method [crm.deal.fields](./crm-deal-fields.md)
||
|| **start**
[`integer`](../../data-types.md)  | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page
||
|#

Also, see the description of [list methods](../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-list.md)

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Get a list of deals where:
1. the funnel ID equals `1`
2. the deal type equals `COMPLEX`
3. the title ends with `a`
4. the stage equals `C1:NEW`
5. the amount is greater than 10000 but less than or equal to 20000
6. manual mode for amount calculation is enabled
7. the responsible person is either the user with `id = 1` or the user with `id = 6`
8. the deal was created at least 6 months ago

Set the following sort order for this selection: title and amount in ascending order.

For clarity, select only the necessary fields:
- Identifier `ID`
- Title `TITLE`
- Deal type `TYPE_ID`
- Funnel ID `CATEGORY_ID`
- Stage `STAGE_ID`
- Amount `OPPORTUNITY`
- Is manual mode enabled `IS_MANUAL_OPPORTUNITY`
- Responsible `ASSIGNED_BY_ID`
- Creation date `DATE_CREATE`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","TITLE","TYPE_ID","CATEGORY_ID","STAGE_ID","OPPORTUNITY","IS_MANUAL_OPPORTUNITY","ASSIGNED_BY_ID","DATE_CREATE"],"FILTER":{"=%TITLE":"%a","CATEGORY_ID":1,"TYPE_ID":"COMPLEX","STAGE_ID":"C1:NEW",">OPPORTUNITY":10000,"<=OPPORTUNITY":20000,"IS_MANUAL_OPPORTUNITY":"Y","@ASSIGNED_BY_ID":[1,6],">DATE_CREATE":"'"$(date --date='-6 months' +%Y-%m-%d)"'"},"ORDER":{"TITLE":"ASC","OPPORTUNITY":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","TITLE","TYPE_ID","CATEGORY_ID","STAGE_ID","OPPORTUNITY","IS_MANUAL_OPPORTUNITY","ASSIGNED_BY_ID","DATE_CREATE"],"FILTER":{"=%TITLE":"%a","CATEGORY_ID":1,"TYPE_ID":"COMPLEX","STAGE_ID":"C1:NEW",">OPPORTUNITY":10000,"<=OPPORTUNITY":20000,"IS_MANUAL_OPPORTUNITY":"Y","@ASSIGNED_BY_ID":[1,6],">DATE_CREATE":"'"$(date --date='-6 months' +%Y-%m-%d)"'"},"ORDER":{"TITLE":"ASC","OPPORTUNITY":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    const now = new Date();
    const sixMonthAgo = new Date();
    sixMonthAgo.setMonth(now.getMonth() - 6);
    
    try {
      const response = await $b24.callListMethod(
        'crm.deal.list',
        {
          select: [
            'ID',
            'TITLE',
            'TYPE_ID',
            'CATEGORY_ID',
            'STAGE_ID',
            'OPPORTUNITY',
            'IS_MANUAL_OPPORTUNITY',
            'ASSIGNED_BY_ID',
            'DATE_CREATE',
          ],
          filter: {
            '=%TITLE': '%а',
            CATEGORY_ID: 1,
            TYPE_ID: 'COMPLEX',
            STAGE_ID: 'C1:NEW',
            '>OPPORTUNITY': 10000,
            '<=OPPORTUNITY': 20000,
            IS_MANUAL_OPPORTUNITY: 'Y',
            '@ASSIGNED_BY_ID': [1, 6],
            '>DATE_CREATE': sixMonthAgo,
          },
          order: {
            TITLE: 'ASC',
            OPPORTUNITY: 'ASC',
          },
        },
        (result) => {
          result.error()
            ? console.error(result.error())
            : console.info(result.data())
          ;
        },
      );
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    const now = new Date();
    const sixMonthAgo = new Date();
    sixMonthAgo.setMonth(now.getMonth() - 6);
    
    try {
      const generator = $b24.fetchListMethod('crm.deal.list', {
        select: [
          'ID',
          'TITLE',
          'TYPE_ID',
          'CATEGORY_ID',
          'STAGE_ID',
          'OPPORTUNITY',
          'IS_MANUAL_OPPORTUNITY',
          'ASSIGNED_BY_ID',
          'DATE_CREATE',
        ],
        filter: {
          '=%TITLE': '%а',
          CATEGORY_ID: 1,
          TYPE_ID: 'COMPLEX',
          STAGE_ID: 'C1:NEW',
          '>OPPORTUNITY': 10000,
          '<=OPPORTUNITY': 20000,
          IS_MANUAL_OPPORTUNITY: 'Y',
          '@ASSIGNED_BY_ID': [1, 6],
          '>DATE_CREATE': sixMonthAgo,
        },
        order: {
          TITLE: 'ASC',
          OPPORTUNITY: 'ASC',
        },
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) {
          console.log('Entity:', entity);
        }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    const now = new Date();
    const sixMonthAgo = new Date();
    sixMonthAgo.setMonth(now.getMonth() - 6);
    
    try {
      const response = await $b24.callMethod('crm.deal.list', {
        select: [
          'ID',
          'TITLE',
          'TYPE_ID',
          'CATEGORY_ID',
          'STAGE_ID',
          'OPPORTUNITY',
          'IS_MANUAL_OPPORTUNITY',
          'ASSIGNED_BY_ID',
          'DATE_CREATE',
        ],
        filter: {
          '=%TITLE': '%а',
          CATEGORY_ID: 1,
          TYPE_ID: 'COMPLEX',
          STAGE_ID: 'C1:NEW',
          '>OPPORTUNITY': 10000,
          '<=OPPORTUNITY': 20000,
          IS_MANUAL_OPPORTUNITY: 'Y',
          '@ASSIGNED_BY_ID': [1, 6],
          '>DATE_CREATE': sixMonthAgo,
        },
        order: {
          TITLE: 'ASC',
          OPPORTUNITY: 'ASC',
        },
      }, 0);
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
                'crm.deal.list',
                [
                    'select' => [
                        'ID',
                        'TITLE',
                        'TYPE_ID',
                        'CATEGORY_ID',
                        'STAGE_ID',
                        'OPPORTUNITY',
                        'IS_MANUAL_OPPORTUNITY',
                        'ASSIGNED_BY_ID',
                        'DATE_CREATE',
                    ],
                    'filter' => [
                        '=%TITLE'              => '%а',
                        'CATEGORY_ID'          => 1,
                        'TYPE_ID'              => 'COMPLEX',
                        'STAGE_ID'             => 'C1:NEW',
                        '>OPPORTUNITY'         => 10000,
                        '<=OPPORTUNITY'        => 20000,
                        'IS_MANUAL_OPPORTUNITY' => 'Y',
                        '@ASSIGNED_BY_ID'      => [1, 6],
                        '>DATE_CREATE'         => $sixMonthAgo,
                    ],
                    'order' => [
                        'TITLE'       => 'ASC',
                        'OPPORTUNITY' => 'ASC',
                    ],
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
        echo 'Error fetching deal list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const now = new Date();
    const sixMonthAgo = new Date();
    sixMonthAgo.setMonth(now.getMonth() - 6);
    
    BX24.callMethod(
        'crm.deal.list',
        {
            select: [
                'ID',
                'TITLE',
                'TYPE_ID',
                'CATEGORY_ID',
                'STAGE_ID',
                'OPPORTUNITY',
                'IS_MANUAL_OPPORTUNITY',
                'ASSIGNED_BY_ID',
                'DATE_CREATE',
            ],
            filter: {
                '=%TITLE': '%а',
                CATEGORY_ID: 1,
                TYPE_ID: 'COMPLEX',
                STAGE_ID: 'C1:NEW',
                '>OPPORTUNITY': 10000,
                '<=OPPORTUNITY': 20000,
                IS_MANUAL_OPPORTUNITY: 'Y',
                '@ASSIGNED_BY_ID': [1, 6],
                '>DATE_CREATE': sixMonthAgo,
            },
            order: {
                TITLE: 'ASC',
                OPPORTUNITY: 'ASC',
            },
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $sixMonthAgo = (new DateTime())->modify('-6 months')->format('Y-m-d');

    $result = CRest::call(
        'crm.deal.list',
        [
            'SELECT' => [
                'ID',
                'TITLE',
                'TYPE_ID',
                'CATEGORY_ID',
                'STAGE_ID',
                'OPPORTUNITY',
                'IS_MANUAL_OPPORTUNITY',
                'ASSIGNED_BY_ID',
                'DATE_CREATE',
            ],
            'FILTER' => [
                '=%TITLE' => '%а',
                'CATEGORY_ID' => 1,
                'TYPE_ID' => 'COMPLEX',
                'STAGE_ID' => 'C1:NEW',
                '>OPPORTUNITY' => 10000,
                '<=OPPORTUNITY' => 20000,
                'IS_MANUAL_OPPORTUNITY' => 'Y',
                '@ASSIGNED_BY_ID' => [1, 6],
                '>DATE_CREATE' => $sixMonthAgo,
            ],
            'ORDER' => [
                'TITLE' => 'ASC',
                'OPPORTUNITY' => 'ASC',
            ],
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
            "ID": "37",
            "TITLE": "[A] Deal",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "19999.99",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "1",
            "DATE_CREATE": "2024-09-02T18:37:18+02:00"
        },
        {
            "ID": "38",
            "TITLE": "[A] Deal",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "20000.00",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "6",
            "DATE_CREATE": "2024-09-02T18:37:38+02:00"
        },
        {
            "ID": "39",
            "TITLE": "[B] Sale",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "12500.00",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "1",
            "DATE_CREATE": "2024-04-09T23:11:01+02:00"
        },
        {
            "ID": "40",
            "TITLE": "[B] Deal",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "13500.00",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "6",
            "DATE_CREATE": "2024-08-08T19:00:14+02:00"
        },
        {
            "ID": "41",
            "TITLE": "[C] Deal",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "11500.00",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "6",
            "DATE_CREATE": "2024-05-08T09:38:23+02:00"
        },
        {
            "ID": "42",
            "TITLE": "[D] Deal",
            "TYPE_ID": "COMPLEX",
            "CATEGORY_ID": "1",
            "STAGE_ID": "C1:NEW",
            "OPPORTUNITY": "18500.00",
            "IS_MANUAL_OPPORTUNITY": "Y",
            "ASSIGNED_BY_ID": "6",
            "DATE_CREATE": "2024-07-02T15:38:32+02:00"
        }
    ],
    "total": 6,
    "time": {
        "start": 1725292115.026221,
        "finish": 1725292115.907058,
        "duration": 0.8808369636535645,
        "processing": 0.2484450340270996,
        "date_start": "2024-09-02T17:48:35+02:00",
        "date_finish": "2024-09-02T17:48:35+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`deal[]`](crm-deal-get.md#deal) | The root element of the response. Contains an array of objects with information about the fields of deals. 

It should be noted that the structure of fields may change due to the `select` parameter ||
|| **total**
[`integer`](../../data-types.md) | The total number of found items ||
|| **next**
[`integer`](../../data-types.md) | Contains the value that needs to be passed in the next request in the `start` parameter to get the next batch of data.

The `next` parameter appears in the response if the number of items matching your request exceeds `50` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'filter' must be array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | `Access denied` | The user does not have permission to "read" deals ||
|| `-`     | `Parameter 'order' must be array` | A non-object was passed to the `order` parameter ||
|| `-`     | `Parameter 'filter' must be array` | A non-object was passed to the `filter` parameter ||
|| `-`     | `Failed to get list. General error` | An unknown error occurred ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-add.md)
- [{#T}](./crm-deal-update.md)
- [{#T}](./crm-deal-get.md)
- [{#T}](./crm-deal-delete.md)
- [{#T}](./crm-deal-fields.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-objects-with-crm-mode.md)


# Get a list of deals crm.deal.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

> Method name: **crm.deal.list**
> 
> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "read" access permission for deals.

The method `crm.deal.list` returns a list of deals based on a filter. It is an implementation of the list method for deals.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | List of fields that should be populated for the deals in the selection.

You can use masks in the selection:
- `'*'` - to select all fields (excluding custom and multiple fields)
- `'UF_*'` - to select all custom fields (excluding multiple fields)

You can find the list of available fields for selection using the method [`crm.deal.fields`](crm-deal-fields.md).

By default, all fields are taken - `'*'` + Custom fields - `'UF_*'`
||
|| **filter**
[`object`](../../data-types.md) |
Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where
- `field_n` — the name of the field by which the selection of elements will be filtered
- `value_n` — the filter value

You can add a prefix to the keys `field_n` to specify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal

{% note warning %}

The LIKE filter does not work with fields of type `crm_status`, `crm_contact`, `crm_company`. (Deal type (`TYPE_ID`), Stage (`STAGE_ID`), etc.)

{% endnote %}

You can find the list of available fields for filtering using the method [`crm.deal.fields`](crm-deal-fields.md).
||
|| **order**
[`object`](../../data-types.md) |
Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where
- `field_n` — the name of the field by which the selection of contacts will be sorted
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

You can find the list of available fields for sorting using the method [`crm.deal.fields`](crm-deal-fields.md).
||
|| **start**
[`integer`](../../data-types.md)  | This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` — the number of the desired page
||
|#

Also, see the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-list.md)

{% endnote %}


## Code Examples

**Get a list of deals where:**
1. The funnel ID is `1`
2. The deal type is **COMPLEX**
3. The title ends with `a`
4. The stage is **C1:NEW**
5. The amount is greater than 10000 and less than or equal to 20000
6. Manual mode for amount calculation is enabled
7. The responsible person is either the user with `id = 1` or the user with `id = 6`
8. The creation date is at least 6 months ago

**Set the following sort order for this selection:**
* Title and amount in ascending order

**For clarity, let's select only the fields we need:**
* Identifier `ID`
* Title `TITLE`
* Deal type `TYPE_ID`
* Funnel ID `CATEGORY_ID`
* Stage `STAGE_ID`
* Amount `OPPORTUNITY`
* Is manual mode enabled `IS_MANUAL_OPPORTUNITY`
* Responsible `ASSIGNED_BY_ID`
* Creation date `DATE_CREATE`


{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    todo
    ```

- cURL (OAuth)

    ```bash
    todo
    ```

- JS

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
                '=%TITLE': '%a',
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

- PHP

    ```php
    todo
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
[`deal[]`](crm-deal-get.md#deal) | The root element of the response. Contains an array of objects with information about the fields of the deals. Note that the structure of the fields may change due to the `select` parameter. ||
|| **total**
[`integer`](../../data-types.md) | The total number of found elements. ||
|| **next**
[`integer`](../../data-types.md) | Contains the value that should be passed in the next request in the `start` parameter to get the next batch of data.

The `next` parameter appears in the response if the number of elements matching your request exceeds `50`. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
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
|| `-`     | Access denied. | The user does not have permission to "read" deals. ||
|| `-`     | Parameter 'order' must be array. | The `order` parameter is not an object. ||
|| `-`     | Parameter 'filter' must be array. | The `filter` parameter is not an object. ||
|| `-`     | Failed to get list. General error. | An unknown error occurred. ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}


## Continue Learning

TODO
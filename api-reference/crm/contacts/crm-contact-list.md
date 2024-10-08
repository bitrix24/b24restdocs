# Get a List of Contacts crm.contact.list

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "read" access permission for contacts

The method `crm.contact.list` returns a list of contacts based on a filter. It is an implementation of the list method for contacts.

To get a list of companies associated with a contact, use the method [`crm.contact.company.items.get`](company/crm-contact-company-items-get.md)

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`][1] | A list of fields that should be filled for the contacts in the selection.

You can use masks in the selection:
- `'*'` — to select all fields (excluding custom and multiple fields)
- `'UF_*'` — to select all custom fields (excluding multiple fields)

There is no mask for selecting multiple fields. To select multiple fields, specify the required ones in the selection list (`PHONE`, `EMAIL`, etc.).

You can find the list of available fields for selection using the method [`crm.contact.fields`](crm-contact-fields.md).

By default, all fields are taken — `'*'` + Custom fields — `'UF_*'`
||
|| **filter**
[`object`][1] | Object format:

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
- `%` — LIKE, substring search. The `%` character in the filter value does not need to be passed. The search looks for a substring in any position of the string
- `=%` — LIKE, substring search. The `%` character needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The fields Phone (`PHONE`), Email (`EMAIL`), Website (`WEB`), Messengers (`IM`), Links (`LINK`) — are multiple. Filters for them only work on exact matches.

Also, the `LIKE` filter does not work with fields of type `crm_status`, `crm_contact`, `crm_company` — for example, Contact Type (`TYPE_ID`), Honorific (`HONORIFIC`), etc.

You can find the list of available fields for filtering using the method [`crm.contact.fields`](crm-contact-fields.md)
||
|| **order**
[`object`][1] | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where:
- `field_n` — the name of the field by which the selection of contacts will be sorted
- `value_n` — a `string` value equal to:
    - `ASC` — ascending order
    - `DESC` — descending order

You can find the list of available fields for sorting using the method [`crm.contact.fields`](crm-contact-fields.md)
||
|| **start**
[`integer`][1] | Parameter for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` — the desired page number
||
|#

Also, see the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

Get a list of contacts where:
1. the source is CRM Form
2. first name and last name are not empty
3. first name or last name starts with "I"
4. are participating in the export
5. e-mail equals 'special-for@example.com'
6. the responsible ID is either 1 or 6
7. created less than 6 months ago

Set the following sort order for this selection: first name and last name in ascending order.

For clarity, select only the necessary fields:
- Contact ID
- First Name
- Last Name
- E-mail
- Participation in Export
- Responsible
- Creation Date

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FILTER":{"SOURCE_ID":"CRM_FORM","!=NAME":"","!=LAST_NAME":"","LOGIC":"OR","0":{"=%NAME":"I%"},"1":{"=%LAST_NAME":"I%"},"EMAIL":"special-for@example.com","@ASSIGNED_BY_ID":[1,6],"IMPORT":"Y",">=DATE_CREATE":"**put_six_month_ago_date_here**"},"ORDER":{"LAST_NAME":"ASC","NAME":"ASC"},"SELECT":["ID","NAME","LAST_NAME","EMAIL","EXPORT","ASSIGNED_BY_ID","DATE_CREATE"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FILTER":{"SOURCE_ID":"CRM_FORM","!=NAME":"","!=LAST_NAME":"","LOGIC":"OR","0":{"=%NAME":"I%"},"1":{"=%LAST_NAME":"I%"},"EMAIL":"special-for@example.com","@ASSIGNED_BY_ID":[1,6],"IMPORT":"Y",">=DATE_CREATE":"**put_six_month_ago_date_here**"},"ORDER":{"LAST_NAME":"ASC","NAME":"ASC"},"SELECT":["ID","NAME","LAST_NAME","EMAIL","EXPORT","ASSIGNED_BY_ID","DATE_CREATE"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.list
    ```

- JS

    ```js
    const sixMonthAgo = new Date();
    sixMonthAgo.setMonth((new Date()).getMonth() - 6);

    BX24.callMethod(
        'crm.contact.list',
        {
            filter: {
                "SOURCE_ID": "CRM_FORM",
                "!=NAME": "",
                "!=LAST_NAME": "",
                "0": {
                    "LOGIC": "OR",
                    "0": {
                        "=%NAME": "I%",
                    },
                    "1": {
                        "=%LAST_NAME": "I%",
                    },
                },
                "EMAIL": "special-for@example.com",
                "@ASSIGNED_BY_ID": [1, 6],
                "IMPORT": "Y",
                ">=DATE_CREATE": sixMonthAgo.toISOString(),
            },
            order: {
                LAST_NAME: "ASC",
                NAME: "ASC",
            },
            select: [
                "ID",
                "NAME",
                "LAST_NAME",
                "EMAIL",
                "EXPORT",
                "ASSIGNED_BY_ID",
                "DATE_CREATE",
            ],
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
    require_once('crest.php');

    $sixMonthAgo = new DateTime();
    $sixMonthAgo->modify('-6 months');

    $result = CRest::call(
        'crm.contact.list',
        [
            'FILTER' => [
                'SOURCE_ID' => 'CRM_FORM',
                '!=NAME' => '',
                '!=LAST_NAME' => '',
                'LOGIC' => 'OR',
                [
                    '=%NAME' => 'I%',
                ],
                [
                    '=%LAST_NAME' => 'I%',
                ],
                'EMAIL' => 'special-for@example.com',
                '@ASSIGNED_BY_ID' => [1, 6],
                'IMPORT' => 'Y',
                '>=DATE_CREATE' => $sixMonthAgo->format(DateTime::ATOM),
            ],
            'ORDER' => [
                'LAST_NAME' => 'ASC',
                'NAME' => 'ASC',
            ],
            'SELECT' => [
                'ID',
                'NAME',
                'LAST_NAME',
                'EMAIL',
                'EXPORT',
                'ASSIGNED_BY_ID',
                'DATE_CREATE',
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
	"result": [
		{
			"ID": "75",
			"NAME": "Anastasia",
			"LAST_NAME": "Ilina",
			"EXPORT": "Y",
			"ASSIGNED_BY_ID": "6",
			"DATE_CREATE": "2024-02-26T00:00:00+02:00",
			"EMAIL": [
				{
					"ID": "215",
					"VALUE_TYPE": "WORK",
					"VALUE": "special-for@example.com",
					"TYPE_ID": "EMAIL"
				}
			]
		},
		{
			"ID": "74",
			"NAME": "Artem",
			"LAST_NAME": "Isaev",
			"EXPORT": "Y",
			"ASSIGNED_BY_ID": "1",
			"DATE_CREATE": "2024-08-15T00:00:00+02:00",
			"EMAIL": [
				{
					"ID": "214",
					"VALUE_TYPE": "WORK",
					"VALUE": "special-for@example.com",
					"TYPE_ID": "EMAIL"
				}
			]
		},
		{
			"ID": "78",
			"NAME": "Artem",
			"LAST_NAME": "Isaev",
			"EXPORT": "Y",
			"ASSIGNED_BY_ID": "1",
			"DATE_CREATE": "2024-08-15T00:00:00+02:00",
			"EMAIL": [
				{
					"ID": "218",
					"VALUE_TYPE": "WORK",
					"VALUE": "special-for@example.com",
					"TYPE_ID": "EMAIL"
				}
			]
		},
		{
			"ID": "77",
			"NAME": "Inna",
			"LAST_NAME": "Kuznetsova",
			"EXPORT": "Y",
			"ASSIGNED_BY_ID": "6",
			"DATE_CREATE": "2024-07-01T00:00:00+02:00",
			"EMAIL": [
				{
					"ID": "217",
					"VALUE_TYPE": "WORK",
					"VALUE": "special-for@example.com",
					"TYPE_ID": "EMAIL"
				}
			]
		},
		{
			"ID": "73",
			"NAME": "Ivan",
			"LAST_NAME": "Petrov",
			"EXPORT": "Y",
			"ASSIGNED_BY_ID": "1",
			"DATE_CREATE": "2024-02-20T00:00:00+02:00",
			"EMAIL": [
				{
					"ID": "213",
					"VALUE_TYPE": "WORK",
					"VALUE": "special-for@example.com",
					"TYPE_ID": "EMAIL"
				}
			]
		}
	],
	"total": 5,
	"time": {
		"start": 1723807142.916445,
		"finish": 1723807143.44846,
		"duration": 0.5320150852203369,
		"processing": 0.1967020034790039,
		"date_start": "2024-08-16T13:19:02+02:00",
		"date_finish": "2024-08-16T13:19:03+02:00"
	}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`contact[]`](./crm-contact-get.md#contact) | The root element of the response. An array containing information about the found contacts.

The fields of an individual contact are configured by the `select` parameter ||
|| **total**
[`integer`][1] | The total number of contacts found based on the specified conditions ||
|| **next**
[`integer`][1] | Contains the value that should be passed in the next request in the `start` parameter to get the next batch of data.

The `next` parameter appears in the response if the number of elements matching your request exceeds `50` ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

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
|| `-`     | `Access denied` | The user does not have permission for "Read" contacts ||
|| `-`     | `Parameter 'order' must be array` | A non-array value was passed to the `order` parameter ||
|| `-`     | `Parameter 'filter' must be array` | A non-array value was passed to the `filter` parameter ||
|| `-`     | `Failed to get list. General error` | An unknown error occurred ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-add.md)
- [{#T}](./crm-contact-update.md)
- [{#T}](./crm-contact-get.md)
- [{#T}](./crm-contact-delete.md)
- [{#T}](./crm-contact-fields.md)

[1]: ../../data-types.md
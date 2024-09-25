# Get a List of Custom Fields for Contacts crm.contact.userfield.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.contact.userfield.list` returns a list of custom fields for contacts based on a filter.

It also outputs information about these fields, but without the name assigned by the user, only the internal identifier. If you need the custom field name, use the method [crm.contact.list](../crm-contact-list.md), which outputs both standard and custom fields.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
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
- `field_n` — the name of the field by which the selection of custom fields will be filtered
- `value_n` — the filter value (only exact matches are available)

All conditions for individual fields are combined using `AND`. See the [list of available fields for filtering](#filterable) below ||
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
- `field_n` — the name of the field by which the selection of elements will be sorted
- `value_n` — a `string` value, equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

Available fields for sorting:
- `ID` — identifier of the custom field
- `FIELD_NAME` — code of the custom field
- `USER_TYPE_ID` — type of the custom field
- `XML_ID` — external code
- `SORT` — sort index

By default:

```json
{
    "SORT": "ASC",
    "ID": "ASC"
}
```
||
|#

### Available Fields for Filtering {#filterable}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`][1] | Identifier of the custom field ||
|| **FIELD_NAME**
[`string`][1] | Code of the custom field ||
|| **USER_TYPE_ID**
[`string`][1] | Type of the custom field. Possible values:
- `string` — string
- `integer` — integer
- `double` — number
- `boolean` — yes/no
- `datetime` — date/time
- `date` — date
- `money` — money
- `url` — link
- `address` — address
- `enumeration` — list
- `file` — file
- `employee` — link to employee
- `crm_status` — link to CRM directory
- `iblock_section` — link to information block sections
- `iblock_element` — link to information block elements
- `crm` — link to CRM elements
- [custom field types](../../universal/user-defined-field-types/index.md)
||
|| **XML_ID**
[`string`][1] | External code ||
|| **SORT**
[`integer`][1] | Sort index ||
|| **MULTIPLE**
[`boolean`][1] | Is the custom field multiple (`Y` — yes / `N` — no) ||
|| **MANDATORY**
[`boolean`][1] | Is the custom field mandatory (`Y` — yes / `N` — no) ||
|| **SHOW_FILTER**
[`char`][1] | Should it be shown in the list filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring
||
|| **SHOW_IN_LIST**
[`boolean`][1] | Should it be shown in the list (`Y` — yes/ `N` — no).

This parameter does not affect anything within `crm`
||
|| **EDIT_IN_LIST**
[`boolean`][1] | Allow user editing (`Y` — yes/ `N` — no) ||
|| **IS_SEARCHABLE**
[`boolean`][1] | Are the field values included in the search (`Y` — yes/ `N` — no)

This parameter does not affect anything within `crm`
||
|| **LANG**
[`string`][1] | [Language identifier](../../data-types.md#last-ids). When filtering by this parameter, a set of fields with values in the specified language will be provided:
- `EDIT_FORM_LABEL` — label in the edit form
- `LIST_COLUMN_LABEL` — header in the list
- `LIST_FILTER_LABEL` — filter label in the list
- `ERROR_MESSAGE` — error message
- `HELP_MESSAGE` — help
||
|#

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

Get a list of custom fields that:
1. are multiple
2. are mandatory
3. have custom field labels in German

Set the following sort order for this selection: field type and sort index in ascending order.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"MULTIPLE":"Y","MANDATORY":"Y","LANG":"de"},"order":{"USER_TYPE_ID":"ASC","SORT":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/crm.contact.userfield.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"MULTIPLE":"Y","MANDATORY":"Y","LANG":"de"},"order":{"USER_TYPE_ID":"ASC","SORT":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.userfield.list
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.contact.userfield.list',
        {
            filter: {
                MULTIPLE: "Y",
                MANDATORY: "Y",
                LANG: "de",
            },
            order: {
                USER_TYPE_ID: "ASC",
                SORT: "ASC",
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
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.userfield.list',
        [
            'filter' => [
                'MULTIPLE' => "Y",
                'MANDATORY' => "Y",
                'LANG' => "de",
            ],
            'order' => [
                'USER_TYPE_ID' => "ASC",
                'SORT' => "ASC",
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
        "ID": "474",
        "ENTITY_ID": "CRM_CONTACT",
        "FIELD_NAME": "UF_CRM_1724412832",
        "USER_TYPE_ID": "address",
        "XML_ID": null,
        "SORT": "300",
        "MULTIPLE": "Y",
        "MANDATORY": "Y",
        "SHOW_FILTER": "E",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "SHOW_MAP": "Y"
        },
        "EDIT_FORM_LABEL": "Custom Field (Address)",
        "LIST_COLUMN_LABEL": "Custom Field (Address)",
        "LIST_FILTER_LABEL": "Custom Field (Address)",
        "ERROR_MESSAGE": null,
        "HELP_MESSAGE": null
        },
        {
        "ID": "475",
        "ENTITY_ID": "CRM_CONTACT",
        "FIELD_NAME": "UF_CRM_1724412867",
        "USER_TYPE_ID": "crm",
        "XML_ID": null,
        "SORT": "1400",
        "MULTIPLE": "Y",
        "MANDATORY": "Y",
        "SHOW_FILTER": "I",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "CONTACT": "Y",
            "COMPANY": "Y",
            "DYNAMIC_1224": "Y",
            "DYNAMIC_1226": "Y",
            "DYNAMIC_1268": "Y",
            "DYNAMIC_1278": "Y",
            "LEAD": null
        },
        "EDIT_FORM_LABEL": "Custom Field (Link to CRM Elements)",
        "LIST_COLUMN_LABEL": "Custom Field (Link to CRM Elements)",
        "LIST_FILTER_LABEL": "Custom Field (Link to CRM Elements)",
        "ERROR_MESSAGE": null,
        "HELP_MESSAGE": null
        },
        {
        "ID": "472",
        "ENTITY_ID": "CRM_CONTACT",
        "FIELD_NAME": "UF_CRM_1724412764",
        "USER_TYPE_ID": "date",
        "XML_ID": null,
        "SORT": "2000",
        "MULTIPLE": "Y",
        "MANDATORY": "Y",
        "SHOW_FILTER": "E",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "DEFAULT_VALUE": {
            "VALUE": "2024-08-22",
            "TYPE": "FIXED"
            }
        },
        "EDIT_FORM_LABEL": "Custom Field (Date)",
        "LIST_COLUMN_LABEL": "Custom Field (Date)",
        "LIST_FILTER_LABEL": "Custom Field (Date)",
        "ERROR_MESSAGE": null,
        "HELP_MESSAGE": null
        },
        {
        "ID": "471",
        "ENTITY_ID": "CRM_CONTACT",
        "FIELD_NAME": "UF_CRM_1724412713",
        "USER_TYPE_ID": "double",
        "XML_ID": null,
        "SORT": "1500",
        "MULTIPLE": "Y",
        "MANDATORY": "Y",
        "SHOW_FILTER": "E",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "PRECISION": 2,
            "SIZE": 20,
            "MIN_VALUE": 0,
            "MAX_VALUE": 0,
            "DEFAULT_VALUE": 150
        },
        "EDIT_FORM_LABEL": "Custom Field (Number)",
        "LIST_COLUMN_LABEL": "Custom Field (Number)",
        "LIST_FILTER_LABEL": "Custom Field (Number)",
        "ERROR_MESSAGE": null,
        "HELP_MESSAGE": null
        },
        {
        "ID": "473",
        "ENTITY_ID": "CRM_CONTACT",
        "FIELD_NAME": "UF_CRM_1724412805",
        "USER_TYPE_ID": "employee",
        "XML_ID": null,
        "SORT": "800",
        "MULTIPLE": "Y",
        "MANDATORY": "Y",
        "SHOW_FILTER": "I",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": [],
        "EDIT_FORM_LABEL": "Custom Field (Employee)",
        "LIST_COLUMN_LABEL": "Custom Field (Employee)",
        "LIST_FILTER_LABEL": "Custom Field (Employee)",
        "ERROR_MESSAGE": null,
        "HELP_MESSAGE": null
        },
        {
        "ID": "476",
        "ENTITY_ID": "CRM_CONTACT",
        "FIELD_NAME": "UF_CRM_1724412914",
        "USER_TYPE_ID": "file",
        "XML_ID": null,
        "SORT": "1200",
        "MULTIPLE": "Y",
        "MANDATORY": "Y",
        "SHOW_FILTER": "N",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "SIZE": 20,
            "LIST_WIDTH": 0,
            "LIST_HEIGHT": 0,
            "MAX_SHOW_SIZE": 0,
            "MAX_ALLOWED_SIZE": 0,
            "EXTENSIONS": [],
            "TARGET_BLANK": "Y"
        },
        "EDIT_FORM_LABEL": "Custom Field (File)",
        "LIST_COLUMN_LABEL": "Custom Field (File)",
        "LIST_FILTER_LABEL": "Custom Field (File)",
        "ERROR_MESSAGE": null,
        "HELP_MESSAGE": null
        }
    ],
    "total": 6,
    "time": {
        "start": 1724435524.016968,
        "finish": 1724435527.855702,
        "duration": 3.8387339115142822,
        "processing": 0.366832971572876,
        "date_start": "2024-08-23T19:52:04+02:00",
        "date_finish": "2024-08-23T19:52:07+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`userfield[]`](crm-contact-userfield-get.md#userfield) | Root element of the response, contains a list of custom fields.

The structure of an individual custom field is identical to [`userfield`](./crm-contact-userfield-get.md#userfield) except that the fields: `EDIT_FORM_LABEL`, `LIST_COLUMN_LABEL`, `LIST_FILTER_LABEL`, `ERROR_MESSAGE`, `HELP_MESSAGE` are returned either as `string` when passing `filter.LANG`, or are not returned at all ||
|| **total**
[`integer`][1] | Number of found custom fields ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'filter' must be array."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | `Parameter 'order' must be array` | The provided `order` is not an object ||
|| `-`     | `Parameter 'filter' must be array` | The provided `filter` is not an object ||
|| `-`     | `Access denied` | The user does not have administrative rights ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-userfield-add.md)
- [{#T}](./crm-contact-userfield-update.md)
- [{#T}](./crm-contact-userfield-get.md)
- [{#T}](./crm-contact-userfield-delete.md)

[1]: ../../../data-types.md
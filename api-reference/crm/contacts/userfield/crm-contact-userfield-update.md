# Update Existing Custom Field for Contacts crm.contact.userfield.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: Administrator

The method `crm.contact.userfield.update` updates an existing custom field for contacts.


## Method Parameters

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`][1] | Identifier of the custom field.

Can be obtained using the methods [`crm.contact.userfield.add`](crm-contact-userfield-add.md) and [`crm.contact.userfield.list`](crm-contact-userfield-list.md) ||
|| **fields^*^**
[`object`][1] | Object format
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where
- `field_n` — field name
- `value_n` — new field value

The list of available fields is described [below](#parametr-fields).

An incorrect field in `fields` will be ignored.

{% note info %}

Only the fields that need to be changed should be passed in `fields`

{% endnote %}
||
|#

### Parameter fields

#|
|| **Parameter**
`type` | **Description** ||
|| **MANDATORY**
[`boolean`][1] | Is the field mandatory

Possible values:
* `Y` - Yes
* `N` - No

||
|| **SHOW_FILTER**
[`boolean`][1] | Should the field be shown in the filter

Possible values:
* `Y` - Yes
* `N` - No

||
|| **XML_ID**
[`string`][1] | External code ||
|| **SETTINGS**
[`object`][1] | Additional field parameters. Each field type (`USER_TYPE_ID`) has its own set of available settings, which are described [below](#settings)

The field only overwrites the passed values ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the custom field of type `enumeration`. This parameter is meaningless for custom fields of other types ||
|| **SORT**
[`integer`][1] | Sort index. Must be greater than zero ||
|| **SHOW_IN_LIST**
[`boolean`][1] | Should the custom field be shown in the list

This parameter has no effect within `crm`.

Possible values:
- `Y` - Yes
- `N` - No

||
|| **EDIT_IN_LIST**
[`boolean`][1] | Allow user editing

Possible values:
- `Y` - Yes
- `N` - No

||
|| **IS_SEARCHABLE**
[`boolean`][1] | Are the field values included in the search

This parameter has no effect within `crm`.

Possible values:
- `Y` - Yes
- `N` - No

||
|| **LIST_FILTER_LABEL**
[`string`][1]\|[`lang_map`](../../data-types.md) | Filter label in the list

When passing a string, it is set for each language
For languages where no value is explicitly specified, it will be recorded as `''`

The field completely overwrites the previous value ||
|| **LIST_COLUMN_LABEL**
[`string`][1]\|[`lang_map`](../../data-types.md) | Header in the list

When passing a string, it is set for each language
For languages where no value is explicitly specified, it will be recorded as `''`

The field completely overwrites the previous value ||
|| **EDIT_FORM_LABEL**
[`string`][1]\|[`lang_map`](../../data-types.md) | Label in the edit form

When passing a string, it is set for each language
For languages where no value is explicitly specified, it will be recorded as `''`

The field completely overwrites the previous value ||
|| **ERROR_MESSAGE**
[`string`][1]\|[`lang_map`](../../data-types.md) | Error message

When passing a string, it is set for each language
For languages where no value is explicitly specified, it will be recorded as `''`

The field completely overwrites the previous value ||
|| **HELP_MESSAGE**
[`string`][1]\|[`lang_map`](../../data-types.md) | Help

When passing a string, it is set for each language
For languages where no value is explicitly specified, it will be recorded as `''`

The field completely overwrites the previous value ||
|#

### Parameter SETTINGS {#settings}

{% note info "Support for Custom Fields" %}

Each type of custom field has its own set of additional settings. This method only supports changing those described below.

{% endnote %}

{% list tabs %}

- string

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`string`][1] | Default value ||
  || **ROWS**
  [`integer`][1] | Number of rows in the input field. Must be greater than 0 and less than 50

  If a value <= 0 is passed -> it will be set to `1`
  If a value >= 50 is passed -> it will be set to `50`
  ||
  |#

- integer

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`integer`][1] | Default value ||
  |#

- double

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`double`][1] | Default value ||
  || **PRECISION**
  [`integer`][1] | Number precision. Must be greater than or equal to zero

  If an invalid value is passed, it will be set to `2` ||
  |#

- boolean

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`integer`][1] | Default value, where `1` - Yes / `0` - No

  When passing a value, it will be set according to the rule:
    * `>= 1` -> 1
    * `<= 0` -> 0

  ||
  || **DISPLAY**
  [`string`][1] | Appearance

  Possible values:
    - `CHECKBOX` - Checkbox
    - `RADIO` - Radio buttons
    - `DROPDOWN` - Dropdown list

  ||
  |#

- datetime

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`object`][1]  | Default value

  Object format:
    ```
    {
        VALUE: datetime,
        TYPE: 'NONE'|'NOW'|'FIXED',
    }
    ```

  where
    - `VALUE` - Default value of type `datetime`
    - `TYPE` - Type of default value:
        * `NONE` - Do not set a default value
        * `NOW` - Use current time/date
        * `FIXED` - Use time/date from `VALUE`

  If an invalid value is passed, it will be set to:
    ```
        VALUE: '',
        TYPE: 'NONE',
    ```
  ||
  |#

- enumeration

  #|
  || **Name**
  `type` | **Description** ||
  || **DISPLAY**
  [`string`][1] | Appearance

  Possible values:
    - `LIST` - List
    - `UI` - Input list
    - `CHECKBOX` - Checkboxes
    - `DIALOG` - Entity selection dialog

  ||
  || **LIST_HEIGHT** | Height of the list. Must be greater than 0 ||
  |#

- iblock_section|iblock_element

  #|
  || **Name**
  `type` | **Description** ||
  || **IBLOCK_TYPE_ID**
  [`string`][1] | Identifier of the information block type ||
  || **IBLOCK_ID**
  [`string`][1] | Identifier of the information block ||
  || **DEFAULT_VALUE**
  [`string`][1] | Default value ||
  || **DISPLAY**
  [`string`][1] | Appearance

  Possible values:
    - `DIALOG` - Dialog
    - `UI` - Input list
    - `LIST` - List
    - `CHECKBOX` - Checkboxes

  ||
  || **LIST_HEIGHT**
  [`integer`][1] | Height of the list. Must be greater than 0

  ||
  || **ACTIVE_FILTER**
  [`boolean`][1] | Show elements with the active flag

  Possible values:
    - `Y` - Yes
    - `N` - No

  ||
  |#

- crm_status

  #|
  || **Name**
  `type` | **Description** ||
  || **ENTITY_TYPE**
  [`string`][1] | Identifier of the reference type.

  Use [`crm.status.entity.types`](../../status/crm-status-entity-types.md) to find out possible values. ||
  |#

- crm

  If none of the following options are passed, the binding to leads will be enabled by default (`LEAD = Y`)

  #|
  || **Name**
  `type` | **Description** ||
  || **LEAD**
  [`boolean`][1] | Is the binding to [Leads](../../leads/index.md) enabled?

  Possible values:
    - `Y` - Yes
    - `N` - No

  ||
  || **CONTACT**
  [`boolean`][1] | Is the binding to [Contacts](../index.md) enabled?

  Possible values:
    - `Y` - Yes
    - `N` - No

  ||
  || **COMPANY**
  [`boolean`][1] | Is the binding to [Companies](../../companies/index.md) enabled?

  Possible values:
    - `Y` - Yes
    - `N` - No

  ||
  || **DEAL**
  [`boolean`][1] | Is the binding to [Deals](../../deals/index.md) enabled?

  Possible values:
    - `Y` - Yes
    - `N` - No

  ||
  |#


{% endlist %}

### Type uf_enum_element {#uf_enum_element}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`string`][1] | Identifier of the list element. When this parameter is passed, the corresponding list element will be modified; otherwise, a new list element will be added.

Can be found using the method [`crm.contact.userfield.get`](crm-contact-userfield-get.md#uf_enum_element)
||
|| **DEL**
[`boolean`][1] | Flag required to delete a list element. Makes sense only when passing `ID`.

Possible values:
`Y` - Delete
`N` - Do not delete

Default - `N`
||
|| **VALUE**
[`string`][1] | Value of the list element. ||
|| **SORT**
[`integer`][1] | Sort index. Must be greater than or equal to zero ||
|| **DEF**
[`boolean`][1] | Is the list element the default value?

Possible values:
- `Y` - Yes
- `N` - No

For multiple fields, multiple `DEF = Y` is allowed. For non-multiple fields, the default value will be considered the first passed list element with `DEF = Y` ||
|| **XML_ID**
[`string`][1] | External code of the value. Must be unique within the elements of the custom field list ||
|#


## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

### Example of Updating a Custom Field of Type String

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
    BX24.callMethod(
        'crm.contact.userfield.update',
        {
            id: 536,
            fields: {
                MANDATORY: "N",
                SHOW_FILTER: "N",
                SETTINGS: {
                    DEFAULT_VALUE: "Hello, World! Default value (changed)",
                    ROWS: 10,
                },
                SORT: 2000,
                EDIT_IN_LIST: "N",
                LIST_FILTER_LABEL: "Hello, World! Filter (changed)",
                LIST_COLUMN_LABEL: {
                    "en": "Hello, World! Column (changed)",
                    "de": "Hallo, Welt! Spalte (geändert)"
                },
                EDIT_FORM_LABEL: {
                    "en": "Hello, World! Edit (changed)",
                    "de": "Hallo, Welt! Bearbeiten (geändert)"
                },
                ERROR_MESSAGE: {
                    "en": "Hello, World! Error (changed)",
                    "de": "Hallo, Welt! Fehler (geändert)"
                },
                HELP_MESSAGE: {
                    "en": "Hello, World! Help (changed)",
                    "de": "Hallo, Welt! Hilfe (geändert)"
                },
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

### Example of Updating a Custom Field of Type List

Current list elements:
```json
[
  {
    "ID": "115",
    "SORT": "100",
    "VALUE": "List Item #1",
    "DEF": "Y",
    "XML_ID": "XML_ID_1"
  },
  {
    "ID": "116",
    "SORT": "200",
    "VALUE": "List Item #2",
    "DEF": "N",
    "XML_ID": "XML_ID_2"
  },
  {
    "ID": "117",
    "SORT": "300",
    "VALUE": "List Item #3",
    "DEF": "N",
    "XML_ID": "XML_ID_3"
  },
  {
    "ID": "118",
    "SORT": "400",
    "VALUE": "List Item #4",
    "DEF": "N",
    "XML_ID": "XML_ID_4"
  }
]
```

We will change it as follows:
* Remove list items with `ID = 115` and `ID = 116`
* Modify the list item with `ID  = 117`:
    * `VALUE`: "List Item #3" -> "List Item #3 (changed)"
    * `SORT`: 300 -> 50
* Add a new list item "List Item #5"

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
    BX.rest.callMethod(
        'crm.contact.userfield.update',
        {
            fields: {
                MANDATORY: "N",
                SHOW_FILTER: "Y",
                LIST: [
                    {
                        ID: 115,
                        DEL: "Y"
                    },
                    {
                        ID: 116,
                        DEL: "Y",
                    },
                    {
                        ID: 117,
                        VALUE: "List Item #3 (changed)",
                        SORT: 50,
                    },
                    {
                        VALUE: "List Item #5",
                        XML_ID: "XML_ID_5",
                        SORT: 500,
                    },
                ],
                SETTINGS: {
                    DISPLAY: "DIALOG",
                    LIST_HEIGHT: 3,
                },
                SORT: 1000,
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
	"result": true,
	"time": {
		"start": 1724419843.518672,
		"finish": 1724419844.120328,
		"duration": 0.6016559600830078,
		"processing": 0.1907808780670166,
		"date_start": "2024-08-23T15:30:43+02:00",
		"date_finish": "2024-08-23T15:30:44+02:00",
		"operating": 0
	}
}
```

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | Parameter 'fields' must be array. | The passed `fields` is not an object ||
|| `-`     | ID is not defined or invalid.     | The passed `id` is less than zero or not passed at all ||
|| `-`     | Access denied.                    | Occurs in cases:
* The user does not have administrative rights
* The user is trying to delete a custom field not linked to contacts ||

|| `ERROR_NOT_FOUND` | The entity with ID '`id`' is not found. | The custom field with the passed `id` does not exist ||
|| `ERROR_CORE`               | List element with value XML_ID=`XML_ID` already exists. | The passed `XML_ID` for the list element must be unique within the elements of a given custom field ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}


## Continue Learning

TODO

[1]: ../../../data-types.md
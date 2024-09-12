# Create a Custom Field for Contacts crm.contact.userfield.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: Administrator

The method `crm.contact.userfield.add` creates a new custom field for contacts.


## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **fields^*^**
[`object`][1] | Format object.

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
- `value_n` — field value

The list of available fields is described [below](#parametr-fields).

An incorrect field in `fields` will be ignored ||
|#

### Parameter fields

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **USER_TYPE_ID^*^**
[`string`][1] | Data type of the custom field.

Possible values:
- `string` - String
- `integer` - Integer
- `double` - Number
- `boolean` - Yes/No
- `datetime` - Date/Time
- `date` - Date
- `money` - Money
- `url` - Link
- `address` - Address
- `enumeration` - List
- `file` - File
- `employee` - Employee binding
- `crm_status` - CRM directory binding
- `iblock_section` - Binding to information block sections
- `iblock_element` - Binding to information block elements
- `crm` - Binding to CRM elements
- [Custom field types](../../universal/user-defined-field-types/index.md)

||
|| **FIELD_NAME^*^**
[`string`][1] | Field code. Unique.

There is a system limitation on the field code of 20 characters. The prefix `UF_CRM_` is always added to the custom field name, meaning the actual length of the name is 13 characters.
Allowed characters: A-Z, 0-9, and _.
||
|| **LABEL**
[`string`][1] | Default name of the custom field.

The provided value will be set in the following fields: `LIST_FILTER_LABEL`, `LIST_COLUMN_LABEL`, `EDIT_FORM_LABEL`, `ERROR_MESSAGE`, `HELP_MESSAGE` if no value is provided for them ||
|| **XML_ID**
[`string`][1] | External code ||
|| **LIST_FILTER_LABEL**
[`string`][1]\|[`lang_map`](../../data-types.md) | Filter label in the list

When a string is provided, it will be set for all language identifiers.
When a value of type `lang_map` is provided, the value from `LABEL` will be set for all languages not provided.

By default - The value provided in `LABEL` is set for all language identifiers ||
|| **LIST_COLUMN_LABEL**
[`string`][1]\|[`lang_map`](../../data-types.md) | Header in the list

When a string is provided, it will be set for all language identifiers.
When a value of type `lang_map` is provided, the value from `LABEL` will be set for all languages not provided.

By default - The value provided in `LABEL` is set for all language identifiers ||
|| **EDIT_FORM_LABEL**
[`string`][1]\|[`lang_map`](../../data-types.md) | Label in the edit form

When a string is provided, it will be set for all language identifiers.
When a value of type `lang_map` is provided, the value from `LABEL` will be set for all languages not provided.

By default - The value provided in `LABEL` is set for all language identifiers ||
|| **ERROR_MESSAGE**
[`string`][1]\|[`lang_map`](../../data-types.md) | Error message

When a string is provided, it will be set for all language identifiers.
When a value of type `lang_map` is provided, the value from `LABEL` will be set for all languages not provided.

By default - The value provided in `LABEL` is set for all language identifiers ||
|| **HELP_MESSAGE**
[`string`][1]\|[`lang_map`](../../data-types.md) | Help

When a string is provided, it will be set for all language identifiers.
When a value of type `lang_map` is provided, the value from `LABEL` will be set for all languages not provided.

By default - The value provided in `LABEL` is set for all language identifiers ||
|| **MULTIPLE**
[`boolean`][1] | Is the field multiple

Possible values:
* `Y` - Yes
* `N` - No

Fields of type `boolean` cannot be multiple

By default - `N` ||
|| **MANDATORY**
[`boolean`][1] | Is the field mandatory

Possible values:
* `Y` - Yes
* `N` - No

By default - `N` ||
|| **SHOW_FILTER**
[`boolean`][1] | Show the field in the filter

Possible values:
* `Y` - Yes
* `N` - No

By default - `N` ||
|| **SETTINGS**
[`object`][1] | Additional field parameters. Each field type (`USER_TYPE_ID`) has its own set of available settings, which are described [below](#settings) ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the custom field of type `enumeration`. For custom fields of other types, this parameter is meaningless

By default - `[]` ||
|| **SORT**
[`integer`][1] | Sort index. Must be greater than zero

By default - `100` ||
|| **SHOW_IN_LIST**
[`boolean`][1] | Show the custom field in the list

This parameter has no effect within `crm`.

Possible values:
- `Y` - Yes
- `N` - No

By default - `N` ||
|| **EDIT_IN_LIST**
[`boolean`][1] | Allow user editing

Possible values:
- `Y` - Yes
- `N` - No

By default - `Y` ||
|| **IS_SEARCHABLE**
[`boolean`][1] | Are the field values searchable

This parameter has no effect within `crm`.

Possible values:
- `Y` - Yes
- `N` - No

By default - `N` ||
|#

### Parameter SETTINGS {#settings}

{% note info "Support for custom fields" %}

Each type of custom field has its own set of additional settings. This method only supports those described below.

{% endnote %}

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`][1] | Default value

    By default - `''` ||
    || **ROWS**
    [`integer`][1] | Number of rows in the input field. Must be greater than 0

    By default - `1` ||
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

    By default - `2` ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`][1] | Default value, where `1` - Yes / `0` - No

    Possible values:
    * `>= 1` -> 1
    * `<= 0` -> 0

    By default - `0` ||
    || **DISPLAY**
    [`string`][1] | Appearance

    Possible values:
    - `CHECKBOX` - Checkbox
    - `RADIO` - Radio buttons
    - `DROPDOWN` - Dropdown list

    By default - `CHECKBOX` ||
    |#

- date|datetime

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`object`][1]  | Default value

    Format object:
    ```
    {
        VALUE: datetime|date,
        TYPE: 'NONE'|'NOW'|'FIXED',
    }
    ```

    where
    - `VALUE` - Default value of type `datetime` or `date`
    - `TYPE` - Type of default value:
      * `NONE` - Do not set a default value
      * `NOW` - Use current time/date
      * `FIXED` - Use time/date from `VALUE`

    Default value:
    ```
    {
        VALUE: '',
        TYPE: 'NONE',
    }
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

    By default - `LIST` ||
    || **LIST_HEIGHT** | Height of the list. Must be greater than 0

    Available only when `DISPLAY = LIST` or `DISPLAY = UI`

    By default `1` ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **IBLOCK_TYPE_ID**
    [`string`][1] | Identifier of the information block type

    By default - `''` ||
    || **IBLOCK_ID**
    [`string`][1] | Identifier of the information block

    By default - `0` ||
    || **DEFAULT_VALUE**
    [`string`][1] | Default value

    By default - `''` ||
    || **DISPLAY**
    [`string`][1] | Appearance

    Possible values:
    - `DIALOG` - Dialog
    - `UI` - Input list
    - `LIST` - List
    - `CHECKBOX` - Checkboxes

    By default - `LIST` ||
    || **LIST_HEIGHT**
    [`integer`][1] | Height of the list. Must be greater than 0

    By default - `1` ||
    || **ACTIVE_FILTER**
    [`boolean`][1] | Show elements with the active flag

    Possible values:
    - `Y` - Yes
    - `N` - No

    By default - `N` ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
    || **ENTITY_TYPE**
    [`string`][1] | Identifier of the directory type.

    Use [`crm.status.entity.types`](../../status/crm-status-entity-types.md) to find possible values.

    By default - `''` ||
    |#

- crm

    If none of the following options are provided, binding to leads will be enabled by default (`LEAD = Y`)

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`boolean`][1] | Is binding to [Leads](../../leads/index.md) enabled?

    Possible values:
    - `Y` - Yes
    - `N` - No

    By default - `N` ||
    || **CONTACT**
    [`boolean`][1] | Is binding to [Contacts](../index.md) enabled?

    Possible values:
    - `Y` - Yes
    - `N` - No

    By default - `N` ||
    || **COMPANY**
    [`boolean`][1] | Is binding to [Companies](../../companies/index.md) enabled?

    Possible values:
    - `Y` - Yes
    - `N` - No

    By default - `N` ||
    || **DEAL**
    [`boolean`][1] | Is binding to [Deals](../../deals/index.md) enabled?

    Possible values:
    - `Y` - Yes
    - `N` - No

    By default - `N` ||
    |#

{% endlist %}

### Type uf_enum_element {#uf_enum_element}

#|
|| **Parameter**
`type` | **Description** ||
|| **VALUE**
[`string`][1] | Value of the list item.

List items with an empty or missing `VALUE` will be ignored ||
|| **SORT**
[`integer`][1] | Sort index. Must be greater than or equal to zero

By default - `0` ||
|| **DEF**
[`boolean`][1] | Is the list item the default value?

Possible values:
- `Y` - Yes
- `N` - No

For multiple fields, multiple `DEF = Y` is allowed. For non-multiple fields, the first provided list item with `DEF = Y` will be considered the default value.

By default - `N` ||
|| **XML_ID**
[`string`][1] | External code of the value. Must be unique within the list items of the custom field ||
|#


## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

### Example of Creating a Custom Field of Type String

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
        'crm.contact.userfield.add',
        {
            fields: {
                LABEL: "Field 'Hello, World!'",
                USER_TYPE_ID: "string",
                FIELD_NAME: "HELLO_WORLD",
                MULTIPLE: "Y",
                MANDATORY: "Y",
                SHOW_FILTER: "Y",
                SETTINGS: {
                    DEFAULT_VALUE: "Hello, World! Default value",
                    ROWS: 3,
                },
                SORT: 1000,
                EDIT_IN_LIST: "Y",
                LIST_FILTER_LABEL: "Hello, World! Filter",
                LIST_COLUMN_LABEL: {
                    "en": "Hello, World! Column",
                    "ru": "Hello, World! Column",
                    "de": "Hallo, Welt! Spalte"
                },
                EDIT_FORM_LABEL: {
                    "en": "Hello, World! Edit",
                    "ru": "Hello, World! Edit",
                    "de": "Hallo, Welt! Bearbeiten"
                },
                ERROR_MESSAGE: {
                    "en": "Hello, World! Error",
                    "ru": "Hello, World! Error",
                    "de": "Hallo, Welt! Fehler"
                },
                HELP_MESSAGE: {
                    "en": "Hello, World! Help",
                    "ru": "Hello, World! Help",
                    "de": "Hallo, Welt! Hilfe"
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

### Example of Creating a Custom Field of Type List

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
        'crm.contact.userfield.add',
        {
            fields: {
                LABEL: "Custom Field (List)",
                USER_TYPE_ID: "enumeration",
                FIELD_NAME: "ENUMERATION_EXAMPLE",
                MULTIPLE: "N",
                MANDATORY: "N",
                SHOW_FILTER: "Y",
                LIST: [
                    {
                        VALUE: "List item #1",
                        DEF: "Y",
                        XML_ID: "XML_ID_1",
                        SORT: 100,
                    },
                    {
                        VALUE: "List item #2",
                        XML_ID: "XML_ID_2",
                        SORT: 200,
                    },
                    {
                        VALUE: "List item #3",
                        XML_ID: "XML_ID_3",
                        SORT: 300,
                    },
                    {
                        VALUE: "List item #4",
                        XML_ID: "XML_ID_4",
                        SORT: 400,
                    },
                ],
                SETTINGS: {
                    DISPLAY: "UI",
                    LIST_HEIGHT: 2,
                },
                SORT: 2000,
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

HTTP Status: **200**

```json
{
	"result": 399,
	"time": {
		"start": 1724239307.903115,
		"finish": 1724239308.567422,
		"duration": 0.6643068790435791,
		"processing": 0.20090818405151367,
		"date_start": "2024-08-21T13:21:47+02:00",
		"date_finish": "2024-08-21T13:21:48+02:00",
		"operating": 0
	}
}
```

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`][1] | Root element of the response, contains the identifier of the created custom field ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

{% note info "Errors" %}

This method may return errors not immediately, but by collecting several, concatenating them with the string: `\n`

{% endnote %}

HTTP Status: **400**

```json
{
	"error": "",
	"error_description": "The 'USER_TYPE_ID' field is not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Description** | **Value** ||
|| Access denied. | The user does not have administrative rights ||
|| The 'FIELD_NAME' field is not found. | Either an empty `FIELD_NAME` was provided, or it was not provided at all ||
|| The field name is too long (more than 50 characters). | The provided `FIELD_NAME` contains more than 50 characters ||
|| The field name contains invalid characters. Allowed are: A-Z, 0-9, and _. | The provided `FIELD_NAME` contains invalid characters ||
|| The 'USER_TYPE_ID' field is not found. | Either an empty `USER_TYPE_ID` was provided, or it was not provided at all ||
|| An invalid user type was specified. | The provided `USER_TYPE_ID` does not exist ||
|| A list item with the value XML_ID=`XML_ID` already exists. | The provided list item `XML_ID` values are not unique ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

TODO

[1]: ../../../data-types.md
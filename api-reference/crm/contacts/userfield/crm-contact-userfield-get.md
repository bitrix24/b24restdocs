# Get Custom Contact Field by Id crm.contact.userfield.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: Administrator

The method `crm.contact.userfield.get` returns a custom field of contacts by its identifier.


## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`][1] | Identifier of the custom field associated with the contact

Can be obtained using the methods [`crm.contact.userfield.add`](crm-contact-userfield-add.md) or [`crm.contact.userfield.list`](crm-contact-userfield-list.md) ||
|#


## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of retrieving a custom field with `id = 399`

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
        'crm.contact.userfield.get',
        {
            id: 399,
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

### Response Handling

HTTP status: **200**

```json
{
	"result": {
		"ID": "399",
		"ENTITY_ID": "CRM_CONTACT",
		"FIELD_NAME": "UF_CRM_HELLO_WORLD",
		"USER_TYPE_ID": "string",
		"XML_ID": null,
		"SORT": "1000",
		"MULTIPLE": "Y",
		"MANDATORY": "Y",
		"SHOW_FILTER": "E",
		"SHOW_IN_LIST": "Y",
		"EDIT_IN_LIST": "Y",
		"IS_SEARCHABLE": "Y",
		"SETTINGS": {
			"SIZE": 20,
			"ROWS": 3,
			"REGEXP": "",
			"MIN_LENGTH": 0,
			"MAX_LENGTH": 0,
			"DEFAULT_VALUE": "Hello, World! Default value"
		},
		"EDIT_FORM_LABEL": {
			"ar": "Field \u0027Hello, World!\u0027",
			"br": "Field \u0027Hello, World!\u0027",
			"en": "Hello, World! Edit",
			"fr": "Field \u0027Hello, World!\u0027",
			"id": "Field \u0027Hello, World!\u0027",
			"it": "Field \u0027Hello, World!\u0027",
			"ja": "Field \u0027Hello, World!\u0027",
			"la": "Field \u0027Hello, World!\u0027",
			"ms": "Field \u0027Hello, World!\u0027",
			"pl": "Field \u0027Hello, World!\u0027",
			"ru": "Hello, World! Edit",
			"sc": "Field \u0027Hello, World!\u0027",
			"tc": "Field \u0027Hello, World!\u0027",
			"th": "Field \u0027Hello, World!\u0027",
			"tr": "Field \u0027Hello, World!\u0027",
			"vn": "Field \u0027Hello, World!\u0027"
		},
		"LIST_COLUMN_LABEL": {
			"ar": "Field \u0027Hello, World!\u0027",
			"br": "Field \u0027Hello, World!\u0027",
			"en": "Hello, World! Column",
			"fr": "Field \u0027Hello, World!\u0027",
			"id": "Field \u0027Hello, World!\u0027",
			"it": "Field \u0027Hello, World!\u0027",
			"ja": "Field \u0027Hello, World!\u0027",
			"la": "Field \u0027Hello, World!\u0027",
			"ms": "Field \u0027Hello, World!\u0027",
			"pl": "Field \u0027Hello, World!\u0027",
			"ru": "Hello, World! Column",
			"sc": "Field \u0027Hello, World!\u0027",
			"tc": "Field \u0027Hello, World!\u0027",
			"th": "Field \u0027Hello, World!\u0027",
			"tr": "Field \u0027Hello, World!\u0027",
			"vn": "Field \u0027Hello, World!\u0027"
		},
		"LIST_FILTER_LABEL": {
			"ar": "Hello, World! Filter",
			"br": "Hello, World! Filter",
			"en": "Hello, World! Filter",
			"fr": "Hello, World! Filter",
			"id": "Hello, World! Filter",
			"it": "Hello, World! Filter",
			"ja": "Hello, World! Filter",
			"la": "Hello, World! Filter",
			"ms": "Hello, World! Filter",
			"pl": "Hello, World! Filter",
			"ru": "Hello, World! Filter",
			"sc": "Hello, World! Filter",
			"tc": "Hello, World! Filter",
			"th": "Hello, World! Filter",
			"tr": "Hello, World! Filter",
			"vn": "Hello, World! Filter"
		},
		"ERROR_MESSAGE": {
			"ar": "Field \u0027Hello, World!\u0027",
			"br": "Field \u0027Hello, World!\u0027",
			"en": "Hello, World! Error",
			"fr": "Field \u0027Hello, World!\u0027",
			"id": "Field \u0027Hello, World!\u0027",
			"it": "Field \u0027Hello, World!\u0027",
			"ja": "Field \u0027Hello, World!\u0027",
			"la": "Field \u0027Hello, World!\u0027",
			"ms": "Field \u0027Hello, World!\u0027",
			"pl": "Field \u0027Hello, World!\u0027",
			"ru": "Hello, World! Error",
			"sc": "Field \u0027Hello, World!\u0027",
			"tc": "Field \u0027Hello, World!\u0027",
			"th": "Field \u0027Hello, World!\u0027",
			"tr": "Field \u0027Hello, World!\u0027",
			"vn": "Field \u0027Hello, World!\u0027"
		},
		"HELP_MESSAGE": {
			"ar": "Field \u0027Hello, World!\u0027",
			"br": "Field \u0027Hello, World!\u0027",
			"en": "Hello, World! Help",
			"fr": "Field \u0027Hello, World!\u0027",
			"id": "Field \u0027Hello, World!\u0027",
			"it": "Field \u0027Hello, World!\u0027",
			"ja": "Field \u0027Hello, World!\u0027",
			"la": "Field \u0027Hello, World!\u0027",
			"ms": "Field \u0027Hello, World!\u0027",
			"pl": "Field \u0027Hello, World!\u0027",
			"ru": "Hello, World! Help",
			"sc": "Field \u0027Hello, World!\u0027",
			"tc": "Field \u0027Hello, World!\u0027",
			"th": "Field \u0027Hello, World!\u0027",
			"tr": "Field \u0027Hello, World!\u0027",
			"vn": "Field \u0027Hello, World!\u0027"
		}
	},
	"time": {
		"start": 1724318753.341079,
		"finish": 1724318753.621247,
		"duration": 0.2801680564880371,
		"processing": 0.023567914962768555,
		"date_start": "2024-08-22T11:25:53+02:00",
		"date_finish": "2024-08-22T11:25:53+02:00",
		"operating": 0
	}
}
```

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`userfield`](#userfield) | Root element of the response, contains information about the custom field ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### userfield

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`][1] | Identifier of the custom field ||
|| **ENTITY_ID**
[`string`][1] | String identifier linking the custom field to the entity. For methods `crm.contact.userfield.*`, the value `CRM_CONTACT` is automatically assigned ||
|| **FIELD_NAME**
[`string`][1] | Field code. Unique. ||
|| **USER_TYPE_ID**
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
- `employee` - Link to employee
- `crm_status` - Link to CRM directory
- `iblock_section` - Link to information block sections
- `iblock_element` - Link to information block elements
- `crm` - Link to CRM elements
- [Custom field types](../../universal/user-defined-field-types/index.md)

||
|| **XML_ID**
[`string`][1] | External code ||
|| **SORT**
[`integer`][1] | Sort index. ||
|| **MULTIPLE**
[`boolean`][1] | Is the field multiple

Possible values:
* `Y` - Yes
* `N` - No

||
|| **MANDATORY**
[`boolean`][1] | Is the field mandatory

Possible values:
* `Y` - Yes
* `N` - No

||
|| **SHOW_FILTER**
[`boolean`][1] | Show the field in the filter

Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring

||
|| **SHOW_IN_LIST**
[`boolean`][1] | Show the custom field in the list

This parameter does not affect anything within `crm`.

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
[`boolean`][1] | Are the field values searchable

This parameter does not affect anything within `crm`.

Possible values:
- `Y` - Yes
- `N` - No

||
|| **SETTINGS**
[`object`][1] | Additional field parameters. Each field type (`USER_TYPE_ID`) has its own set of available settings, described [below](#settings) ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the custom field of type `enumeration`. For custom fields of other types, this parameter is meaningless ||
|| **EDIT_FORM_LABEL**
[`lang_map`](../../data-types.md) | Label in the edit form ||
|| **LIST_COLUMN_LABEL**
[`lang_map`](../../data-types.md) | Header in the list ||
|| **LIST_FILTER_LABEL**
[`lang_map`](../../data-types.md) | Filter label in the list ||
|| **ERROR_MESSAGE**
[`lang_map`](../../data-types.md) | Error message ||
|| **HELP_MESSAGE**
[`lang_map`](../../data-types.md) | Help ||
|| **USER_TYPE_OWNER**
[`string`][1] | `CLIENT_ID` of the REST application that serves this field type

Returned when the field type is custom. ||
|#

### Parameter settings {#settings}

{% list tabs %}

- double

  #|
  || **Name**
  `type` | **Description** ||
  || **PRECISION**
  [`integer`][1] | Precision (number of decimal places) ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **MIN_VALUE**
  [`double`][1] | Minimum value (0 - do not check) ||
  || **MAX_VALUE**
  [`double`][1] | Maximum value (0 - do not check) ||
  || **DEFAULT_VALUE**
  [`double`][1] | Default value ||
  |#

- boolean

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`integer`][1] | Default value
  `0` - No
  `1` - Yes ||
  || **DISPLAY**
  [`string`][1] | Appearance

  Possible values:
  `CHECKBOX` - Checkbox
  `RADIO` - Radio buttons
  `DROPDOWN` - Dropdown list ||
  || **LABEL**
  [`string[]`][1] | Labels for values, where
  * array element with index `0` - Label for value "No"
  * array element with index `1` - Label for value "Yes"

  ||
  || **LABEL_CHECKBOX**
  [`string`][1] | Checkbox label ||
  |#

- date

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`object`][1] | Default value. Object format:
  ```
  {
      TYPE: 'NONE'|'FIXED'|'NONE',
      VALUE: date
  }
  ```

  where `TYPE` - Type of default value:
  `NONE` - None
  `NOW` - Current date
  `FIXED` - Date from the `VALUE`

  `VALUE` is of type `date` ||
  |#

- integer

  #|
  || **Name**
  `type` | **Description** ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **MIN_VALUE**
  [`integer`][1] | Minimum value (0 - do not check) ||
  || **MAX_VALUE**
  [`integer`][1] | Maximum value (0 - do not check) ||
  || **DEFAULT_VALUE**
  [`integer`][1] | Default value ||
  |#

- datetime

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`object`][1] | Default value. Object format:
  ```
  {
      TYPE: 'NONE'|'FIXED'|'NONE',
      VALUE: datetime
  }
  ```

  where `TYPE` - Type of default value:
  `NONE` - None
  `NOW` - Current date and time
  `FIXED` - Date and time from the `VALUE`

  `VALUE` is of type `datetime` ||
  || **USE_SECOND**
  [`boolean`][1] | Use seconds

  Possible values:
  `Y` - Yes
  `N` - No ||
  || **USE_TIMEZONE**
  [`boolean`][1] | Use time zones

  Possible values:
  `Y` - Yes
  `N` - No ||
  |#

- string

  #|
  || **Name**
  `type` | **Description** ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **ROWS**
  [`integer`][1] | Number of lines in the input field ||
  || **REGEXP**
  [`string`][1] | Regular expression for validation ||
  || **MIN_LENGTH**
  [`integer`][1] | Minimum string length (0 - do not check) ||
  || **MAX_LENGTH**
  [`integer`][1] | Maximum string length (0 - do not check) ||
  || **DEFAULT_VALUE**
  [`string`][1] | Default value ||
  |#

- enumeration

  #|
  || **Name**
  `type` | **Description** ||
  || **DISPLAY**
  [`string`][1] | Appearance

  Possible values:
  `LIST` - List
  `CHECKBOX` - Checkboxes
  `UI` - Input list
  `DIALOG` - Entity selection dialog ||
  || **LIST_HEIGHT**
  [`integer`][1] | List height ||
  || **CAPTION_NO_VALUE**
  [`string`][1] | Label when no value is present ||
  || **SHOW_NO_VALUE**
  [`boolean`][1] | Show empty value for mandatory field

  Possible values:
  `Y` - Yes
  `N` - No ||
  |#

- iblock_section|iblock_element

  #|
  || **Name**
  `type` | **Description** ||
  || **DISPLAY**
  [`string`][1] | Appearance

  Possible values:
  `LIST` - List
  `CHECKBOX` - Checkboxes
  `UI` - Input list
  `DIALOG` - Entity selection dialog ||
  || **LIST_HEIGHT**
  [`integer`][1] | List height ||
  || **IBLOCK_ID**
  [`integer`][1] | Identifier of the information block ||
  || **DEFAULT_VALUE**
  [`integer`][1] | Default value ||
  || **ACTIVE_FILTER**
  [`boolean`][1] | Show only active elements

  Possible values:
  `Y` - Yes
  `N` - No ||
  |#

- crm_status

  #|
  || **Name**
  `type` | **Description** ||
  || **ENTITY_TYPE**
  [`object`][1] | CRM directory. Structure similar to the returned elements of the method [`crm.status.entity.types`](../../status/crm-status-entity-types.md) ||
  |#

- crm

  #|
  || **Name**
  `type` | **Description** ||
  || **LEAD**
  [`boolean`][1] | Link to Leads enabled ||
  || **CONTACT**
  [`boolean`][1] | Link to Contacts enabled ||
  || **COMPANY**
  [`boolean`][1] | Link to Companies enabled ||
  || **DEAL**
  [`boolean`][1] | Link to Deals enabled ||
  || **ORDER**
  [`boolean`][1] | Link to Orders enabled ||
  || **QUOTE**
  [`boolean`][1] | Link to Estimates enabled ||
  || **SMART_INVOICE**
  [`boolean`][1] | Link to New Invoices enabled ||
  || **DYNAMIC_...**
  [`boolean`][1] | Link to a specific SPA enabled

  Each such field has the form: `DYNAMIC_{entityTypeId}`, where `entityTypeId` is the identifier of the SPA type to which the link is enabled. ||
  |#

- money

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`string`][1] | Default value

  The value of this field has the format: "**{VALUE}\|{CURRENCY}**", where
  `VALUE` - Default amount of money
  `CURRENCY` - String identifier of the currency

  For example: "**300\|USD**" - 300 dollars

  ||
  |#

- address

  #|
  || **Name**
  `type` | **Description** ||
  || **SHOW_MAP**
  [`boolean`][1] | Show map ||
  |#

- url

  #|
  || **Name**
  `type` | **Description** ||
  || **POPUP**
  [`boolean`][1] | Open in a new window ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **MIN_LENGTH**
  [`integer`][1] | Minimum string length (0 - do not check) ||
  || **MAX_LENGTH**
  [`integer`][1] | Maximum string length (0 - do not check) ||
  || **DEFAULT_VALUE**
  [`string`][1] | Default value ||
  || **ROWS**
  [`integer`][1] | Number of lines in the input field ||
  |#

- file

  #|
  || **Name**
  `type` | **Description** ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **LIST_WIDTH**
  [`integer`][1] | Maximum width for display in the list ||
  || **LIST_HEIGHT**
  [`integer`][1] | Maximum height for display in the list ||
  || **MAX_SHOW_SIZE**
  [`integer`][1] | Maximum allowable size for display in the list (0 - no limit) ||
  || **MAX_ALLOWED_SIZE**
  [`integer`][1] | Maximum allowable file size for upload (0 - do not check) ||
  || **EXTENSIONS**
  [`string[]`][1] | Allowed extensions ||
  || **TARGET_BLANK**
  [`boolean`][1] | Open file in a new tab ||
  |#

{% endlist %}

### Type uf_enum_element {#uf_enum_element}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`][1] | Identifier of the list element. ||
|| **VALUE**
[`string`][1] | Value of the list element. ||
|| **SORT**
[`integer`][1] | Sort index ||
|| **DEF**
[`boolean`][1] | Is the list element the default value

Possible values:
- `Y` - Yes
- `N` - No

||
|#


## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | Access denied. | Occurs in cases:
* The user does not have administrative rights
* The user is trying to access a custom field not linked to contacts ||
|| `-` | ID is not defined or invalid. | The provided `id` is either less than or equal to zero, or not provided at all ||
|| `ERROR_NOT_FOUND` | The entity with ID `'id'` is not found. | The custom field with the provided `id` was not found ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}


## Continue Learning

TODO

[1]: ../../../data-types.md
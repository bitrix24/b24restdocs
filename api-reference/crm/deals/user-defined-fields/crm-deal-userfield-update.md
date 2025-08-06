# Update Existing Custom Field for Deals crm.deal.userfield.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

The method `crm.deal.userfield.update` updates an existing custom field for deals.

## Method Parameters

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field.

The identifier can be obtained using the methods [crm.deal.userfield.add](./crm-deal-userfield-add.md) and [crm.deal.userfield.list](./crm-deal-userfield-list.md) ||
|| **fields***
[`object`](../../../data-types.md) | Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — field name
- `value_n` — new field value

The list of available fields is described [below](#parameter-fields).

An incorrect field in `fields` will be ignored.

Only those fields that need to be changed should be passed in `fields` ||
|#

### Parameter fields {#parameter-fields}

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **MANDATORY**
[`boolean`](../../../data-types.md) | Is the field mandatory? Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_FILTER**
[`boolean`](../../../data-types.md) | Should the field be shown in the filter? Possible values:
- `Y` — yes
- `N` — no ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional field parameters. Each field type `USER_TYPE_ID` has its own set of available settings, which are described [below](#settings).

The field only overwrites the passed values ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the custom field of type `enumeration`, description [below](#uf_enum_element) ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. Must be greater than zero ||
|| **SHOW_IN_LIST**
[`boolean`](../../../data-types.md) | Should the custom field be shown in the list?

This parameter has no effect within `crm`.

Possible values:
- `Y` — yes
- `N` — no ||
|| **EDIT_IN_LIST**
[`boolean`](../../../data-types.md) | Allow user editing? Possible values:
- `Y` — yes
- `N` — no 
Value `N` is not supported by all field types within `crm` ||
|| **IS_SEARCHABLE**
[`boolean`](../../../data-types.md) | Are the field values searchable?

This parameter has no effect within `crm`.

Possible values:
- `Y` — yes
- `N` — no ||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Filter label in the list.

When passing a string, it is set for each language.

For languages without an explicitly specified value, `''` will be recorded.

The field completely overwrites the previous value ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Header in the list.

When passing a string, it is set for each language.

For languages without an explicitly specified value, `''` will be recorded.

The field completely overwrites the previous value ||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Label in the edit form.

When passing a string, it is set for each language.

For languages without an explicitly specified value, `''` will be recorded.

The field completely overwrites the previous value ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Error message ||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Help ||
|#

### Parameter SETTINGS {#settings}

Each type of custom field has its own set of additional settings. This method only supports those described below.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value ||
    || **ROWS**
    [`integer`](../../../data-types.md) | Number of rows in the input field. Must be greater than 0 ||
    |#

- integer

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../../data-types.md) | Default value ||
    |#

- double

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`double`](../../../data-types.md) | Default value ||
    || **PRECISION**
    [`integer`](../../../data-types.md) | Number precision. Must be greater than or equal to 0 ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../../data-types.md) | Default value, where `1` — yes, `0` — no.

    Possible values:
    - `>= 1` -> 1
    - `<= 0` -> 0 ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance. Possible values:
    - `CHECKBOX` — checkbox
    - `RADIO` — radio buttons
    - `DROPDOWN` — dropdown list ||
    |#

- date|datetime

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`object`](../../../data-types.md)  | Default value.
    Object format:
    ```
    {
        VALUE: datetime|date,
        TYPE: 'NONE'|'NOW'|'FIXED',
    }
    ```
    where:
    - `VALUE` — default value of type `datetime` or `date`
    - `TYPE` — type of default value:
      - `NONE` — do not set a default value
      - `NOW` — use the current time/date
      - `FIXED` — use the time/date from `VALUE` ||
    |#

- enumeration

    #|
    || **Name**
    `type` | **Description** ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance. Possible values:
    - `LIST` — list
    - `UI` — input list
    - `CHECKBOX` — checkboxes
    - `DIALOG` — entity selection dialog ||
    || **LIST_HEIGHT** | Height of the list. Must be greater than 0.

    Available only when `DISPLAY = LIST` or `DISPLAY = UI` ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **IBLOCK_TYPE_ID**
    [`string`](../../../data-types.md) | Identifier of the information block type ||
    || **IBLOCK_ID**
    [`string`](../../../data-types.md) | Identifier of the information block ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance. Possible values:
    - `DIALOG` — dialog
    - `UI` — input list
    - `LIST` — list
    - `CHECKBOX` — checkboxes ||
    || **LIST_HEIGHT**
    [`integer`](../../../data-types.md) | Height of the list. Must be greater than 0 ||
    || **ACTIVE_FILTER**
    [`boolean`](../../../data-types.md) | Should elements with the active flag be shown? Possible values:
    - `Y` — yes
    - `N` — no ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
    || **ENTITY_TYPE**
    [`string`](../../../data-types.md) | Identifier of the reference type.

    Use [`crm.status.entity.types`](../../status/crm-status-entity-types.md) to find possible values ||
    |#

- crm

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`boolean`](../../../data-types.md) | Is the binding to [Leads](../../leads/index.md) enabled? Possible values:
    - `Y` — yes
    - `N` — no ||
    || **CONTACT**
    [`boolean`](../../../data-types.md) | Is the binding to [Contacts](../index.md) enabled? Possible values:
    - `Y` — yes
    - `N` — no ||
    || **COMPANY**
    [`boolean`](../../../data-types.md) | Is the binding to [Companies](../../companies/index.md) enabled? Possible values:
    - `Y` — yes
    - `N` — no ||
    || **DEAL**
    [`boolean`](../../../data-types.md) | Is the binding to [Deals](../index.md) enabled? Possible values:
    - `Y` — yes
    - `N` — no ||
    |#

{% endlist %}

### Parameter LIST {#uf_enum_element}

#|
|| **Name**
`type` | **Description** ||
|| **VALUE**
[`string`](../../../data-types.md) | Value of the list element.

List elements with an empty or missing `VALUE` will be ignored ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. Must be greater than or equal to 0 ||
|| **DEF**
[`boolean`](../../../data-types.md) | Is the list element the default value? Possible values:
- `Y` — yes
- `N` — no

For a multiple field, several `DEF = Y` are allowed. For a non-multiple field, the first passed list element with `DEF = Y` will be considered default ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code of the value. Must be unique within the elements of the custom field list ||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

### Example of Updating a String Type Custom Field

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"N","SETTINGS":{"DEFAULT_VALUE":"Hello, World! Default value (changed)","ROWS":10},"SORT":2000,"EDIT_IN_LIST":"N","LIST_FILTER_LABEL":"Hello, World! Filter (changed)","LIST_COLUMN_LABEL":{"en":"Hello, World! Column (changed)","de":"Hallo, Welt! Spalte (geändert)"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit (changed)","de":"Hallo, Welt! Bearbeiten (geändert)"},"ERROR_MESSAGE":{"en":"Hello, World! Error (changed)","de":"Hallo, Welt! Fehler (geändert)"},"HELP_MESSAGE":{"en":"Hello, World! Help (changed)","de":"Hallo, Welt! Hilfe (geändert)"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"N","SETTINGS":{"DEFAULT_VALUE":"Hello, World! Default value (changed)","ROWS":10},"SORT":2000,"EDIT_IN_LIST":"N","LIST_FILTER_LABEL":"Hello, World! Filter (changed)","LIST_COLUMN_LABEL":{"en":"Hello, World! Column (changed)","de":"Hallo, Welt! Spalte (geändert)"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit (changed)","de":"Hallo, Welt! Bearbeiten (geändert)"},"ERROR_MESSAGE":{"en":"Hello, World! Error (changed)","de":"Hallo, Welt! Fehler (geändert)"},"HELP_MESSAGE":{"en":"Hello, World! Help (changed)","de":"Hallo, Welt! Hilfe (geändert)"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.userfield.update
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.deal.userfield.update',
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
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.userfield.update',
        [
            'id' => 536,
            'fields' => [
                'MANDATORY' => "N",
                'SHOW_FILTER' => "N",
                'SETTINGS' => [
                    'DEFAULT_VALUE' => "Hello, World! Default value (changed)",
                    'ROWS' => 10,
                ],
                'SORT' => 2000,
                'EDIT_IN_LIST' => "N",
                'LIST_FILTER_LABEL' => "Hello, World! Filter (changed)",
                'LIST_COLUMN_LABEL' => [
                    'en' => "Hello, World! Column (changed)",
                    'de' => "Hallo, Welt! Spalte (geändert)"
                ],
                'EDIT_FORM_LABEL' => [
                    'en' => "Hello, World! Edit (changed)",
                    'de' => "Hallo, Welt! Bearbeiten (geändert)"
                ],
                'ERROR_MESSAGE' => [
                    'en' => "Hello, World! Error (changed)",
                    'de' => "Hallo, Welt! Fehler (geändert)"
                ],
                'HELP_MESSAGE' => [
                    'en' => "Hello, World! Help (changed)",
                    'de' => "Hallo, Welt! Hilfe (geändert)"
                ],
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
    "result": true,
    "time": {
        "start": 1753790234.592207,
        "finish": 1753790234.762644,
        "duration": 0.17043709754943848,
        "processing": 0.11566615104675293,
        "date_start": "2025-07-29T14:57:14+02:00",
        "date_finish": "2025-07-29T14:57:14+02:00",
        "operating_reset_at": 1753790834,
        "operating": 0.11564803123474121
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `400`     | `Parameter 'fields' must be array` | The passed `fields` is not an object ||
|| `400`     | `ID is not defined or invalid`     | The passed `id` is less than zero or not passed at all ||
|| `403`     | `Access denied`                    | Occurs when:
- the user does not have administrative rights
- the user tries to change a custom field not linked to deals ||
|| `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found` | The custom field with the passed `id` does not exist ||
|| `ERROR_CORE`               | `List element with value XML_ID='XML_ID' already exists` | The passed `XML_ID` for the list element must be unique within the elements of the custom field ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-userfield-add.md)
- [{#T}](./crm-deal-userfield-get.md)
- [{#T}](./crm-deal-userfield-list.md)
- [{#T}](./crm-deal-userfield-delete.md)
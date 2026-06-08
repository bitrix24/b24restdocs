# Update Company User Field crm.company.userfield.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

The method `crm.company.userfield.update` updates an existing company user field.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the user field.

The identifier can be obtained using the methods [crm.company.userfield.add](./crm-company-userfield-add.md) and [crm.company.userfield.list](./crm-company-userfield-list.md) ||
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

{% include [Parameter Note](../../../../_includes/required.md) %}

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
[`object`](../../../data-types.md) | Additional field parameters. Each field type `USER_TYPE_ID` has its own pool of available settings, which are described [below](#settings).

The field only overwrites the passed values ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the user field of type `enumeration`, description [below](#uf_enum_element) ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. Must be greater than zero ||
|| **SHOW_IN_LIST**
[`boolean`](../../../data-types.md) | Should the user field be shown in the list?

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no ||
|| **EDIT_IN_LIST**
[`boolean`](../../../data-types.md) | Allow user editing? Possible values:
- `Y` — yes
- `N` — no 
The value `N` is not supported by all field types within `crm` ||
|| **IS_SEARCHABLE**
[`boolean`](../../../data-types.md) | Are the field values searchable?

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no ||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Filter label in the list.

When a string is passed, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Header in the list.

When a string is passed, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Label in the edit form.

When a string is passed, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Error message ||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Help ||
|#

### Parameter SETTINGS {#settings}

Each type of user field has its own set of additional settings. This method only supports those described below.

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

    Use [`crm.status.entity.types`](../../status/crm-status-entity-types.md) to find out possible values ||
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
    [`boolean`](../../../data-types.md) | Is the binding to [Contacts](../../contacts/index.md) enabled? Possible values:
    - `Y` — yes
    - `N` — no ||
    || **COMPANY**
    [`boolean`](../../../data-types.md) | Is the binding to [Companies](../index.md) enabled? Possible values:
    - `Y` — yes
    - `N` — no ||
    || **DEAL**
    [`boolean`](../../../data-types.md) | Is the binding to [Deals](../../deals/index.md) enabled? Possible values:
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
[`string`](../../../data-types.md) | External code of the value. Must be unique within the elements of the user field list ||
|#

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

### Example of Changing a String Type User Field

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"N","SETTINGS":{"DEFAULT_VALUE":"Hello, World! Default value (changed)","ROWS":10},"SORT":2000,"EDIT_IN_LIST":"N","LIST_FILTER_LABEL":"Hello, World! Filter (changed)","LIST_COLUMN_LABEL":{"en":"Hello, World! Column (changed)","de":"Hallo, Welt! Spalte (geändert)"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit (changed)","de":"Hallo, Welt! Bearbeiten (geändert)"},"ERROR_MESSAGE":{"en":"Hello, World! Error (changed)","de":"Hallo, Welt! Fehler (geändert)"},"HELP_MESSAGE":{"en":"Hello, World! Help (changed)","de":"Hallo, Welt! Hilfe (geändert)"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"N","SETTINGS":{"DEFAULT_VALUE":"Hello, World! Default value (changed)","ROWS":10},"SORT":2000,"EDIT_IN_LIST":"N","LIST_FILTER_LABEL":"Hello, World! Filter (changed)","LIST_COLUMN_LABEL":{"en":"Hello, World! Column (changed)","de":"Hallo, Welt! Spalte (geändert)"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit (changed)","de":"Hallo, Welt! Bearbeiten (geändert)"},"ERROR_MESSAGE":{"en":"Hello, World! Error (changed)","de":"Hallo, Welt! Fehler (geändert)"},"HELP_MESSAGE":{"en":"Hello, World! Help (changed)","de":"Hallo, Welt! Hilfe (geändert)"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.userfield.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.company.userfield.update',
        params: {
          id: 536,
          fields: {
            MANDATORY: 'N',
            SHOW_FILTER: 'N',
            SETTINGS: {
              DEFAULT_VALUE: 'Hello, World! default value (changed)',
              ROWS: 10,
            },
            SORT: 2000,
            EDIT_IN_LIST: 'N',
            LIST_FILTER_LABEL: 'Hello, World! filter (changed)',
            LIST_COLUMN_LABEL: {
              en: 'Hello, World! Column (changed)',
              de: 'Hallo, Welt! Spalte (geändert)',
            },
            EDIT_FORM_LABEL: {
              en: 'Hello, World! Edit (changed)',
              de: 'Hallo, Welt! Bearbeiten (geändert)',
            },
            ERROR_MESSAGE: {
              en: 'Hello, World! Error (changed)',
              de: 'Hallo, Welt! Fehler (geändert)',
            },
            HELP_MESSAGE: {
              en: 'Hello, World! Help (changed)',
              de: 'Hallo, Welt! Hilfe (geändert)',
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Userfield updated:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function updateCompanyUserfield() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.company.userfield.update',
            params: {
              id: 536,
              fields: {
                MANDATORY: 'N',
                SHOW_FILTER: 'N',
                SETTINGS: {
                  DEFAULT_VALUE: 'Hello, World! default value (changed)',
                  ROWS: 10,
                },
                SORT: 2000,
                EDIT_IN_LIST: 'N',
                LIST_FILTER_LABEL: 'Hello, World! filter (changed)',
                LIST_COLUMN_LABEL: {
                  en: 'Hello, World! Column (changed)',
                  de: 'Hallo, Welt! Spalte (geändert)',
                },
                EDIT_FORM_LABEL: {
                  en: 'Hello, World! Edit (changed)',
                  de: 'Hallo, Welt! Bearbeiten (geändert)',
                },
                ERROR_MESSAGE: {
                  en: 'Hello, World! Error (changed)',
                  de: 'Hallo, Welt! Fehler (geändert)',
                },
                HELP_MESSAGE: {
                  en: 'Hello, World! Help (changed)',
                  de: 'Hallo, Welt! Hilfe (geändert)',
                },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Userfield updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCompanyUserfield)
    </script>
    ```

- PHP

    ```php
    try {
        $result = $serviceBuilder
            ->getCRMScope()
            ->companyUserfield()
            ->update(
                536,
                [
                    'MANDATORY' => 'N',
                    'SHOW_FILTER' => 'N',
                    'SETTINGS' => [
                        'DEFAULT_VALUE' => 'Hello, World! Default value (changed)',
                        'ROWS' => 10,
                    ],
                    'SORT' => 2000,
                    'EDIT_IN_LIST' => 'N',
                    'LIST_FILTER_LABEL' => 'Hello, World! Filter (changed)',
                ]
            );

        print($result->isSuccess() ? 'Updated' : 'Failed');
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.company.userfield.update',
        {
            id: 536,
            fields: {
                MANDATORY: 'N',
                SHOW_FILTER: 'N',
                SETTINGS: {
                    DEFAULT_VALUE: 'Hello, World! Default value (changed)',
                    ROWS: 10,
                },
                SORT: 2000,
                EDIT_IN_LIST: 'N',
                LIST_FILTER_LABEL: 'Hello, World! Filter (changed)',
            },
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
        'crm.company.userfield.update',
        [
            'id' => 536,
            'fields' => [
                'MANDATORY' => 'N',
                'SHOW_FILTER' => 'N',
                'SETTINGS' => [
                    'DEFAULT_VALUE' => 'Hello, World! Default value (changed)',
                    'ROWS' => 10,
                ],
                'SORT' => 2000,
                'EDIT_IN_LIST' => 'N',
                'LIST_FILTER_LABEL' => 'Hello, World! Filter (changed)',
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
|| `400`     | Parameter 'fields' must be array | The passed `fields` is not an object ||
|| `400`     | ID is not defined or invalid     | The passed `id` is less than zero or not passed at all ||
|| `403` | Access denied | Occurs when:
- the user does not have administrative rights
- the user tries to change a user field not linked to companies ||
|| `ERROR_NOT_FOUND` | The entity with ID 'id' is not found | The user field with the passed `id` does not exist ||
|| `ERROR_CORE`               | List element with value XML_ID='XML_ID' already exists | The passed `XML_ID` for the list element must be unique within the elements of the user field list ||
|#
{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-userfield-add.md)
- [{#T}](./crm-company-userfield-get.md)
- [{#T}](./crm-company-userfield-list.md)
- [{#T}](./crm-company-userfield-delete.md)

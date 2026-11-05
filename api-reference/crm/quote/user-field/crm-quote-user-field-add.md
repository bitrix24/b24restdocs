# Create a Custom Field for Estimates crm.quote.userfield.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit CRM preferences" permission in CRM

The method `crm.quote.userfield.add` creates a custom field in all estimates in Bitrix24 and returns its ID. The type `USER_TYPE_ID`, the code `FIELD_NAME`, and the multiple flag `MULTIPLE` cannot be changed after the field is created.

In REST 3.0, the field is created by the [crm.quote.field.custom.add](../../rest-v3/fields/crm-entity-field-custom-add-rest-v3.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
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
- `value_n` — field value

The list of available fields is described [below](#parameter-fields).

An incorrect field in `fields` will be ignored ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_TYPE_ID***
[`string`](../../../data-types.md) | Data type of the custom field. The method [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) returns the list of types available in Bitrix24. Possible values:
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
- [custom field types](../../universal/user-defined-fields/userfield-type.md)
||
|| **FIELD_NAME***
[`string`](../../../data-types.md) | Field code, unique among estimate fields. Bitrix24 builds the full name from the code with the prefix `UF_CRM_`:
- `MANAGER_NOTE` becomes `UF_CRM_MANAGER_NOTE`
- `UF_MANAGER_NOTE` becomes `UF_CRM_MANAGER_NOTE` — the prefix `UF_` is replaced with `UF_CRM_`
- `UF_CRM_MANAGER_NOTE` remains unchanged

The field value is written and read by the universal methods [crm.item.update](../../universal/crm-item-update.md) and [crm.item.get](../../universal/crm-item-get.md) with `entityTypeId = 7`. In these methods, the field name is written in camelCase: `UF_CRM_MANAGER_NOTE` becomes `ufCrmManagerNote`. The methods [crm.quote.update](../crm-quote-update.md) and [crm.quote.get](../crm-quote-get.md) work with the full name, but their development has been halted.

The full name is up to `50` characters including the prefix, that is, the code without the prefix is up to `43` characters. If the code is longer, the method returns the error `ERROR_CORE`.

The code allows `A-Z`, `0-9`, and `_`. The method converts lowercase letters to uppercase and returns the error `ERROR_CORE` for any other characters ||
|| **LABEL**
[`string`](../../../data-types.md) | Field name. If `LIST_FILTER_LABEL`, `LIST_COLUMN_LABEL`, `EDIT_FORM_LABEL`, `ERROR_MESSAGE`, or `HELP_MESSAGE` are not provided, the method writes `LABEL` to them.

If `LABEL` is not provided, the full field name is used instead, for example, `UF_CRM_MANAGER_NOTE` ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code ||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Filter label in the list.

What the method writes:
- a string — one label for all languages
- `lang_map` — labels in the provided languages, and the `LABEL` value in the other languages
- the parameter is not provided — the `LABEL` value in all languages ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Header in the list.

What the method writes:
- a string — one label for all languages
- `lang_map` — labels in the provided languages, and the `LABEL` value in the other languages
- the parameter is not provided — the `LABEL` value in all languages ||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Label in the edit form.

What the method writes:
- a string — one label for all languages
- `lang_map` — labels in the provided languages, and the `LABEL` value in the other languages
- the parameter is not provided — the `LABEL` value in all languages ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Error message. A string and a `lang_map` are processed the same way as for `LIST_FILTER_LABEL` ||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Help. A string and a `lang_map` are processed the same way as for `LIST_FILTER_LABEL` ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Whether the field is multiple, that is, whether it can store several values. Possible values:
- `Y` — yes
- `N` — no

Fields of type `boolean` cannot be multiple: if you pass `MULTIPLE = Y`, the method saves `N` without an error.

By default, `N` ||
|| **MANDATORY**
[`char`](../../../data-types.md) | Is the field mandatory. Possible values:
- `Y` — yes
- `N` — no

By default, `N` ||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Show the field in the filter. Possible values:
- `Y` — yes
- `N` — no

By default, `N`.

Instead of `Y`, the method saves the filter mode, which [crm.quote.userfield.get](./crm-quote-user-field-get.md) and [crm.quote.userfield.list](./crm-quote-user-field-list.md) return:
- the `employee` type — `I`, exact match
- the `money`, `url`, `address`, `file` types and [custom types](../../universal/user-defined-fields/userfield-type.md) — always `N`
- other types — `E`, mask ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional field parameters. The set of settings depends on the field type `USER_TYPE_ID`, described [below](#settings) ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the custom field of type `enumeration`, described [below](#uf_enum_element)

By default `[]` ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. Must be greater than zero.

By default `100` ||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Show the field in the list.

The parameter is not used in CRM.

Possible values:
- `Y` — yes
- `N` — no

By default `Y` ||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allow user editing. Possible values:
- `Y` — yes
- `N` — no

By default `Y`. The value `N` is not supported by all field types within `crm` ||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Are the field values included in the search.

The parameter is not used in CRM.

Possible values:
- `Y` — yes
- `N` — no

By default `N` ||
|#

### Parameter SETTINGS {#settings}

The set of settings depends on the field type. The method saves only the settings from the tabs below and skips other `SETTINGS` keys. For the `money`, `url`, `address`, `file`, `employee` types and custom types, the method ignores `SETTINGS` entirely, and the field gets the default type settings.

The method [crm.quote.userfield.get](./crm-quote-user-field-get.md) returns the saved field settings, and the method [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) returns the list of type settings. Only the settings from the tabs below can be passed in `SETTINGS`.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value.

    Default `''` ||
    || **ROWS**
    [`integer`](../../../data-types.md) | Number of lines in the input field. Must be greater than zero.

    Default `1` ||
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
    [`integer`](../../../data-types.md) | Number precision. Must be greater than or equal to 0.

    Default `0` ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../../data-types.md) | Default value: `1` is yes, `0` is no. The method saves a number of `1` or greater as `1`, and a number of `0` or less as `0`.

    Default `0` ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance. Possible values:
    - `CHECKBOX` — checkbox
    - `RADIO` — radio buttons
    - `DROPDOWN` — dropdown list

    Default `CHECKBOX` ||
    |#

- date|datetime

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`object`](../../../data-types.md) | Default value.
    Object format:
    ```
    {
        VALUE: datetime|date,
        TYPE: 'NONE'|'NOW'|'FIXED',
    }
    ```
    where:
    - `VALUE` — default value of type `datetime` or `date` in ISO 8601 format, for example, `2026-10-28T10:00:00+01:00`
    - `TYPE` — default value type:
      - `NONE` — do not set a default value
      - `NOW` — use current time/date
      - `FIXED` — use time/date from `VALUE`

    Default value:
    ```
    {
        VALUE: '',
        TYPE: 'NONE',
    }
    ``` ||
    |#

- enumeration

    #|
    || **Name**
    `type` | **Description** ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance. Possible values:
    - `LIST` — list
    - `UI` — editable list
    - `CHECKBOX` — checkboxes
    - `DIALOG` — item selection dialog

    Default `LIST` ||
    || **LIST_HEIGHT**
    [`integer`](../../../data-types.md) | List height. Must be greater than zero.

    Available only when `DISPLAY = LIST` or `DISPLAY = UI`.

    Default `1` ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **IBLOCK_TYPE_ID**
    [`string`](../../../data-types.md) | Information block type identifier.

    Default `''` ||
    || **IBLOCK_ID**
    [`string`](../../../data-types.md) | Information block identifier.

    Default `0` ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value.

    Default `''` ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance. Possible values:
    - `DIALOG` — dialog
    - `UI` — editable list
    - `LIST` — list
    - `CHECKBOX` — checkboxes

    Default `LIST` ||
    || **LIST_HEIGHT**
    [`integer`](../../../data-types.md) | List height. Must be greater than zero.

    Default `1` ||
    || **ACTIVE_FILTER**
    [`char`](../../../data-types.md) | Whether to show elements with the activity flag enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default `N` ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
    || **ENTITY_TYPE**
    [`string`](../../../data-types.md) | Directory type identifier.

    Use [`crm.status.entity.types`](../../status/crm-status-entity-types.md) to find possible values.

    Default `''` ||
    |#

- crm

    If all bindings are disabled, Bitrix24 enables binding to leads `LEAD = Y`.

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`char`](../../../data-types.md) | Whether binding to [leads](../../leads/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default `N` ||
    || **CONTACT**
    [`char`](../../../data-types.md) | Whether binding to [contacts](../../contacts/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default `N` ||
    || **COMPANY**
    [`char`](../../../data-types.md) | Whether binding to [companies](../../companies/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default `N` ||
    || **DEAL**
    [`char`](../../../data-types.md) | Whether binding to [deals](../../deals/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default `N` ||
    |#

{% endlist %}

### Parameter LIST {#uf_enum_element}

{% note warning %}

If the `LIST` items are not saved, for example, because of a duplicate `XML_ID`, the method returns an error, but the field is already created — without list items. Before calling the method again, find the field with the method [crm.quote.userfield.list](./crm-quote-user-field-list.md) and add the items with the method [crm.quote.userfield.update](./crm-quote-user-field-update.md) or delete the field.

{% endnote %}

#|
|| **Name**
`type` | **Description** ||
|| **VALUE**
[`string`](../../../data-types.md) | Value of the list item.

List items with an empty or missing `VALUE` will be ignored ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. A value of `0` or less is not saved; the item gets `500` instead.

By default `500` ||
|| **DEF**
[`char`](../../../data-types.md) | Is the list item the default value. Possible values:
- `Y` — yes
- `N` — no

A multiple field can have several default values. For a field with `MULTIPLE = N`, the first passed item with `DEF = Y` becomes the default value.

By default `N` ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code of the value. Must be unique within the list items of the custom field ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

### Example of Creating a Custom Field of Type String

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"LABEL":"Hello, World! Field","USER_TYPE_ID":"string","FIELD_NAME":"HELLO_WORLD","MULTIPLE":"Y","MANDATORY":"Y","SHOW_FILTER":"Y","SETTINGS":{"DEFAULT_VALUE":"Hello, World! Default Value","ROWS":3},"SORT":1000,"EDIT_IN_LIST":"Y","LIST_FILTER_LABEL":"Hello, World! Filter","LIST_COLUMN_LABEL":{"en":"Hello, World! Column","de":"Hallo, Welt! Spalte"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit","de":"Hallo, Welt! Bearbeiten"},"ERROR_MESSAGE":{"en":"Hello, World! Error","de":"Hallo, Welt! Fehler"},"HELP_MESSAGE":{"en":"Hello, World! Help","de":"Hallo, Welt! Hilfe"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.userfield.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"LABEL":"Hello, World! Field","USER_TYPE_ID":"string","FIELD_NAME":"HELLO_WORLD","MULTIPLE":"Y","MANDATORY":"Y","SHOW_FILTER":"Y","SETTINGS":{"DEFAULT_VALUE":"Hello, World! Default Value","ROWS":3},"SORT":1000,"EDIT_IN_LIST":"Y","LIST_FILTER_LABEL":"Hello, World! Filter","LIST_COLUMN_LABEL":{"en":"Hello, World! Column","de":"Hallo, Welt! Spalte"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit","de":"Hallo, Welt! Bearbeiten"},"ERROR_MESSAGE":{"en":"Hello, World! Error","de":"Hallo, Welt! Fehler"},"HELP_MESSAGE":{"en":"Hello, World! Help","de":"Hallo, Welt! Hilfe"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.userfield.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'crm.quote.userfield.add',
        params: {
          fields: {
            LABEL: 'Hello, World! Field',
            USER_TYPE_ID: 'string',
            FIELD_NAME: 'HELLO_WORLD',
            MULTIPLE: 'Y',
            MANDATORY: 'Y',
            SHOW_FILTER: 'Y',
            SETTINGS: {
              DEFAULT_VALUE: 'Hello, World! Default value',
              ROWS: 3,
            },
            SORT: 1000,
            EDIT_IN_LIST: 'Y',
            LIST_FILTER_LABEL: 'Hello, World! Filter',
            LIST_COLUMN_LABEL: {
              en: 'Hello, World! Column',
              de: 'Hallo, Welt! Spalte',
            },
            EDIT_FORM_LABEL: {
              en: 'Hello, World! Edit',
              de: 'Hallo, Welt! Bearbeiten',
            },
            ERROR_MESSAGE: {
              en: 'Hello, World! Error',
              de: 'Hallo, Welt! Fehler',
            },
            HELP_MESSAGE: {
              en: 'Hello, World! Help',
              de: 'Hallo, Welt! Hilfe',
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
        console.info('Created userfield ID:', result)
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
      async function addStringQuoteUserfield() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.userfield.add',
            params: {
              fields: {
                LABEL: 'Hello, World! Field',
                USER_TYPE_ID: 'string',
                FIELD_NAME: 'HELLO_WORLD',
                MULTIPLE: 'Y',
                MANDATORY: 'Y',
                SHOW_FILTER: 'Y',
                SETTINGS: {
                  DEFAULT_VALUE: 'Hello, World! Default value',
                  ROWS: 3,
                },
                SORT: 1000,
                EDIT_IN_LIST: 'Y',
                LIST_FILTER_LABEL: 'Hello, World! Filter',
                LIST_COLUMN_LABEL: {
                  en: 'Hello, World! Column',
                  de: 'Hallo, Welt! Spalte',
                },
                EDIT_FORM_LABEL: {
                  en: 'Hello, World! Edit',
                  de: 'Hallo, Welt! Bearbeiten',
                },
                ERROR_MESSAGE: {
                  en: 'Hello, World! Error',
                  de: 'Hallo, Welt! Fehler',
                },
                HELP_MESSAGE: {
                  en: 'Hello, World! Help',
                  de: 'Hallo, Welt! Hilfe',
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
          console.info('Created userfield ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addStringQuoteUserfield)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.quote.userfield.add(
            fields={
                "LABEL": "Hello, world! Field",
                "USER_TYPE_ID": "string",
                "FIELD_NAME": "HELLO_WORLD",
                "MULTIPLE": "Y",
                "MANDATORY": "Y",
                "SHOW_FILTER": "Y",
                "SETTINGS": {
                    "DEFAULT_VALUE": "Hello, world! Default value",
                    "ROWS": 3,
                },
                "SORT": 1000,
                "EDIT_IN_LIST": "Y",
                "LIST_FILTER_LABEL": "Hello, world! Filter",
                "LIST_COLUMN_LABEL": {
                    "en": "Hello, World! Column",
                    "de": "Hallo, Welt! Spalte",
                },
                "EDIT_FORM_LABEL": {
                    "en": "Hello, World! Edit",
                    "de": "Hallo, Welt! Bearbeiten",
                },
                "ERROR_MESSAGE": {
                    "en": "Hello, World! Error",
                    "de": "Hallo, Welt! Fehler",
                },
                "HELP_MESSAGE": {
                    "en": "Hello, World! Help",
                    "de": "Hallo, Welt! Hilfe",
                },
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $userfieldItemFields = [
            'FIELD_NAME' => 'HELLO_WORLD',
            'USER_TYPE_ID' => 'string',
            'SORT' => 1000,
            'MULTIPLE' => 'Y',
            'MANDATORY' => 'Y',
            'SHOW_FILTER' => 'Y',
            'EDIT_IN_LIST' => 'Y',
            'LIST_FILTER_LABEL' => 'Hello, world! Filter',
            'LABEL' => 'Hello, world! Field',
            'LIST_COLUMN_LABEL' => [
                'en' => 'Hello, World! Column',
                'de' => 'Hallo, Welt! Spalte',
            ],
            'EDIT_FORM_LABEL' => [
                'en' => 'Hello, World! Edit',
                'de' => 'Hallo, Welt! Bearbeiten',
            ],
            'ERROR_MESSAGE' => [
                'en' => 'Hello, World! Error',
                'de' => 'Hallo, Welt! Fehler',
            ],
            'HELP_MESSAGE' => [
                'en' => 'Hello, World! Help',
                'de' => 'Hallo, Welt! Hilfe',
            ],
            'SETTINGS' => [
                'DEFAULT_VALUE' => 'Hello, world! Default value',
                'ROWS' => 3,
            ],
        ];

        $result = $serviceBuilder
            ->getCRMScope()
            ->quoteUserfield()
            ->add($userfieldItemFields);

        print($result->getId());
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.userfield.add',
        {
            fields: {
                LABEL: 'Hello, world! Field',
                USER_TYPE_ID: 'string',
                FIELD_NAME: 'HELLO_WORLD',
                MULTIPLE: 'Y',
                MANDATORY: 'Y',
                SHOW_FILTER: 'Y',
                SETTINGS: {
                    DEFAULT_VALUE: 'Hello, world! Default value',
                    ROWS: 3,
                },
                SORT: 1000,
                EDIT_IN_LIST: 'Y',
                LIST_FILTER_LABEL: 'Hello, world! Filter',
                LIST_COLUMN_LABEL: {
                    en: 'Hello, World! Column',
                    de: 'Hallo, Welt! Spalte',
                },
                EDIT_FORM_LABEL: {
                    en: 'Hello, World! Edit',
                    de: 'Hallo, Welt! Bearbeiten',
                },
                ERROR_MESSAGE: {
                    en: 'Hello, World! Error',
                    de: 'Hallo, Welt! Fehler',
                },
                HELP_MESSAGE: {
                    en: 'Hello, World! Help',
                    de: 'Hallo, Welt! Hilfe',
                },
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
        'crm.quote.userfield.add',
        [
            'fields' => [
                'LABEL' => 'Hello, world! Field',
                'USER_TYPE_ID' => 'string',
                'FIELD_NAME' => 'HELLO_WORLD',
                'MULTIPLE' => 'Y',
                'MANDATORY' => 'Y',
                'SHOW_FILTER' => 'Y',
                'SETTINGS' => [
                    'DEFAULT_VALUE' => 'Hello, world! Default value',
                    'ROWS' => 3,
                ],
                'SORT' => 1000,
                'EDIT_IN_LIST' => 'Y',
                'LIST_FILTER_LABEL' => 'Hello, world! Filter',
                'LIST_COLUMN_LABEL' => [
                    'en' => 'Hello, World! Column',
                    'de' => 'Hallo, Welt! Spalte',
                ],
                'EDIT_FORM_LABEL' => [
                    'en' => 'Hello, World! Edit',
                    'de' => 'Hallo, Welt! Bearbeiten',
                ],
                'ERROR_MESSAGE' => [
                    'en' => 'Hello, World! Error',
                    'de' => 'Hallo, Welt! Fehler',
                ],
                'HELP_MESSAGE' => [
                    'en' => 'Hello, World! Help',
                    'de' => 'Hallo, Welt! Hilfe',
                ],
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.quote.userfield.add", b24.Params{
    	"fields": b24.Params{
    		"LABEL":        "Hello, World! Field",
    		"USER_TYPE_ID": "string",
    		"FIELD_NAME":   "HELLO_WORLD",
    		"MULTIPLE":     "Y",
    		"MANDATORY":    "Y",
    		"SHOW_FILTER":  "Y",
    		"SETTINGS": b24.Params{
    			"DEFAULT_VALUE": "Hello, World! Default Value",
    			"ROWS":          3,
    		},
    		"SORT":              1000,
    		"EDIT_IN_LIST":      "Y",
    		"LIST_FILTER_LABEL": "Hello, World! Filter",
    		"LIST_COLUMN_LABEL": b24.Params{
    			"en": "Hello, World! Column",
    			"de": "Hallo, Welt! Spalte",
    		},
    		"EDIT_FORM_LABEL": b24.Params{
    			"en": "Hello, World! Edit",
    			"de": "Hallo, Welt! Bearbeiten",
    		},
    		"ERROR_MESSAGE": b24.Params{
    			"en": "Hello, World! Error",
    			"de": "Hallo, Welt! Fehler",
    		},
    		"HELP_MESSAGE": b24.Params{
    			"en": "Hello, World! Help",
    			"de": "Hallo, Welt! Hilfe",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.quote.userfield.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

### Example of Creating a Custom Field of Type List

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"LABEL":"Custom field (list)","USER_TYPE_ID":"enumeration","FIELD_NAME":"ENUM_EXAMPLE","MULTIPLE":"N","MANDATORY":"N","SHOW_FILTER":"Y","LIST":[{"VALUE":"List item #1","DEF":"Y","XML_ID":"XML_ID_1","SORT":100},{"VALUE":"List item #2","XML_ID":"XML_ID_2","SORT":200},{"VALUE":"List item #3","XML_ID":"XML_ID_3","SORT":300},{"VALUE":"List item #4","XML_ID":"XML_ID_4","SORT":400}],"SETTINGS":{"DISPLAY":"UI","LIST_HEIGHT":2},"SORT":2000}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.userfield.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"LABEL":"Custom field (list)","USER_TYPE_ID":"enumeration","FIELD_NAME":"ENUM_EXAMPLE","MULTIPLE":"N","MANDATORY":"N","SHOW_FILTER":"Y","LIST":[{"VALUE":"List item #1","DEF":"Y","XML_ID":"XML_ID_1","SORT":100},{"VALUE":"List item #2","XML_ID":"XML_ID_2","SORT":200},{"VALUE":"List item #3","XML_ID":"XML_ID_3","SORT":300},{"VALUE":"List item #4","XML_ID":"XML_ID_4","SORT":400}],"SETTINGS":{"DISPLAY":"UI","LIST_HEIGHT":2},"SORT":2000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.userfield.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'crm.quote.userfield.add',
        params: {
          fields: {
            LABEL: 'Custom field (list)',
            USER_TYPE_ID: 'enumeration',
            FIELD_NAME: 'ENUM_EXAMPLE',
            MULTIPLE: 'N',
            MANDATORY: 'N',
            SHOW_FILTER: 'Y',
            LIST: [
              {
                VALUE: 'List item #1',
                DEF: 'Y',
                XML_ID: 'XML_ID_1',
                SORT: 100,
              },
              {
                VALUE: 'List item #2',
                XML_ID: 'XML_ID_2',
                SORT: 200,
              },
              {
                VALUE: 'List item #3',
                XML_ID: 'XML_ID_3',
                SORT: 300,
              },
              {
                VALUE: 'List item #4',
                XML_ID: 'XML_ID_4',
                SORT: 400,
              },
            ],
            SETTINGS: {
              DISPLAY: 'UI',
              LIST_HEIGHT: 2,
            },
            SORT: 2000,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created userfield ID:', result)
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
      async function addEnumerationQuoteUserfield() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.userfield.add',
            params: {
              fields: {
                LABEL: 'Custom field (list)',
                USER_TYPE_ID: 'enumeration',
                FIELD_NAME: 'ENUM_EXAMPLE',
                MULTIPLE: 'N',
                MANDATORY: 'N',
                SHOW_FILTER: 'Y',
                LIST: [
                  {
                    VALUE: 'List item #1',
                    DEF: 'Y',
                    XML_ID: 'XML_ID_1',
                    SORT: 100,
                  },
                  {
                    VALUE: 'List item #2',
                    XML_ID: 'XML_ID_2',
                    SORT: 200,
                  },
                  {
                    VALUE: 'List item #3',
                    XML_ID: 'XML_ID_3',
                    SORT: 300,
                  },
                  {
                    VALUE: 'List item #4',
                    XML_ID: 'XML_ID_4',
                    SORT: 400,
                  },
                ],
                SETTINGS: {
                  DISPLAY: 'UI',
                  LIST_HEIGHT: 2,
                },
                SORT: 2000,
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
          console.info('Created userfield ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addEnumerationQuoteUserfield)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.quote.userfield.add(
            fields={
                "LABEL": "Custom field (list)",
                "USER_TYPE_ID": "enumeration",
                "FIELD_NAME": "ENUM_EXAMPLE",
                "MULTIPLE": "N",
                "MANDATORY": "N",
                "SHOW_FILTER": "Y",
                "LIST": [
                    {
                        "VALUE": "List item #1",
                        "DEF": "Y",
                        "XML_ID": "XML_ID_1",
                        "SORT": 100,
                    },
                    {
                        "VALUE": "List item #2",
                        "XML_ID": "XML_ID_2",
                        "SORT": 200,
                    },
                    {
                        "VALUE": "List item #3",
                        "XML_ID": "XML_ID_3",
                        "SORT": 300,
                    },
                    {
                        "VALUE": "List item #4",
                        "XML_ID": "XML_ID_4",
                        "SORT": 400,
                    },
                ],
                "SETTINGS": {
                    "DISPLAY": "UI",
                    "LIST_HEIGHT": 2,
                },
                "SORT": 2000,
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $userfieldItemFields = [
            'LABEL' => 'Custom field (list)',
            'USER_TYPE_ID' => 'enumeration',
            // The PHP SDK accepts a field code of up to 13 characters
            'FIELD_NAME' => 'ENUM_EXAMPLE',
            'MULTIPLE' => 'N',
            'MANDATORY' => 'N',
            'SHOW_FILTER' => 'Y',
            'LIST' => [
                ['VALUE' => 'List item #1', 'DEF' => 'Y', 'XML_ID' => 'XML_ID_1', 'SORT' => 100],
                ['VALUE' => 'List item #2', 'XML_ID' => 'XML_ID_2', 'SORT' => 200],
                ['VALUE' => 'List item #3', 'XML_ID' => 'XML_ID_3', 'SORT' => 300],
                ['VALUE' => 'List item #4', 'XML_ID' => 'XML_ID_4', 'SORT' => 400],
            ],
            'SETTINGS' => ['DISPLAY' => 'UI', 'LIST_HEIGHT' => 2],
            'SORT' => 2000,
        ];

        $result = $serviceBuilder
            ->getCRMScope()
            ->quoteUserfield()
            ->add($userfieldItemFields);

        print($result->getId());
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.userfield.add',
        {
            fields: {
                LABEL: 'Custom field (list)',
                USER_TYPE_ID: 'enumeration',
                FIELD_NAME: 'ENUM_EXAMPLE',
                MULTIPLE: 'N',
                MANDATORY: 'N',
                SHOW_FILTER: 'Y',
                LIST: [
                    { VALUE: 'List item #1', DEF: 'Y', XML_ID: 'XML_ID_1', SORT: 100 },
                    { VALUE: 'List item #2', XML_ID: 'XML_ID_2', SORT: 200 },
                    { VALUE: 'List item #3', XML_ID: 'XML_ID_3', SORT: 300 },
                    { VALUE: 'List item #4', XML_ID: 'XML_ID_4', SORT: 400 },
                ],
                SETTINGS: { DISPLAY: 'UI', LIST_HEIGHT: 2 },
                SORT: 2000,
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
        'crm.quote.userfield.add',
        [
            'fields' => [
                'LABEL' => 'Custom field (list)',
                'USER_TYPE_ID' => 'enumeration',
                'FIELD_NAME' => 'ENUM_EXAMPLE',
                'MULTIPLE' => 'N',
                'MANDATORY' => 'N',
                'SHOW_FILTER' => 'Y',
                'LIST' => [
                    ['VALUE' => 'List item #1', 'DEF' => 'Y', 'XML_ID' => 'XML_ID_1', 'SORT' => 100],
                    ['VALUE' => 'List item #2', 'XML_ID' => 'XML_ID_2', 'SORT' => 200],
                    ['VALUE' => 'List item #3', 'XML_ID' => 'XML_ID_3', 'SORT' => 300],
                    ['VALUE' => 'List item #4', 'XML_ID' => 'XML_ID_4', 'SORT' => 400],
                ],
                'SETTINGS' => [
                    'DISPLAY' => 'UI',
                    'LIST_HEIGHT' => 2,
                ],
                'SORT' => 2000,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.quote.userfield.add", b24.Params{
    	"fields": b24.Params{
    		"LABEL":        "Custom field (list)",
    		"USER_TYPE_ID": "enumeration",
    		"FIELD_NAME":   "ENUM_EXAMPLE",
    		"MULTIPLE":     "N",
    		"MANDATORY":    "N",
    		"SHOW_FILTER":  "Y",
    		"LIST": []b24.Params{
    			{
    				"VALUE":  "List item #1",
    				"DEF":    "Y",
    				"XML_ID": "XML_ID_1",
    				"SORT":   100,
    			},
    			{
    				"VALUE":  "List item #2",
    				"XML_ID": "XML_ID_2",
    				"SORT":   200,
    			},
    			{
    				"VALUE":  "List item #3",
    				"XML_ID": "XML_ID_3",
    				"SORT":   300,
    			},
    			{
    				"VALUE":  "List item #4",
    				"XML_ID": "XML_ID_4",
    				"SORT":   400,
    			},
    		},
    		"SETTINGS": b24.Params{
    			"DISPLAY":     "UI",
    			"LIST_HEIGHT": 2,
    		},
    		"SORT": 2000,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.quote.userfield.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 6997,
    "time": {
        "start": 1753789240.8146,
        "finish": 1753789241.058695,
        "duration": 0.2440950870513916,
        "processing": 0.19217395782470703,
        "date_start": "2025-07-29T14:40:40+03:00",
        "date_finish": "2025-07-29T14:40:41+03:00",
        "operating_reset_at": 1753789840,
        "operating": 0.19216084480285645
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Root element of the response, contains the identifier of the created custom field ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The 'USER_TYPE_ID' field is not found."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

If both `USER_TYPE_ID` and `FIELD_NAME` are not provided, both errors are returned in one `error_description`, separated by a line break.

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Access denied.` | The user does not have the "Edit CRM preferences" permission in CRM ||
|| `400` | Empty value | `The 'FIELD_NAME' field is not found.` | `FIELD_NAME` is not provided or is empty ||
|| `400` | `ERROR_CORE` | `The field name is too long (exceeds 50 symbols).` | The full field name including the prefix `UF_CRM_` contains more than 50 characters, that is, the code without the prefix is longer than 43 characters ||
|| `400` | `ERROR_CORE` | `The field name contains invalid symbols. Valid symbols are: A-Z, 0-9 and _.` | The provided `FIELD_NAME` contains characters other than `A-Z`, `0-9`, and `_` ||
|| `400` | Empty value | `The 'USER_TYPE_ID' field is not found.` | `USER_TYPE_ID` is not provided or is empty ||
|| `400` | `ERROR_CORE` | `The custom type is invalid.` | The provided `USER_TYPE_ID` does not exist ||
|| `400` | `ERROR_CORE` | `The field UF_CRM_CODE for entity CRM_QUOTE already exists.` | An estimate field with this `FIELD_NAME` already exists ||
|| `400` | `ERROR_CORE` | `List element with XML_ID=xml_id already exists.` | `XML_ID` is repeated in the `LIST` items ||
|| `500` | `INTERNAL_SERVER_ERROR` | Depends on the cause | Estimates already have 1016 custom fields, or the field creation did not complete within `max_execution_time`. For details, see [Errors When Working with Custom Fields](./index.md#errors) ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-user-field-update.md)
- [{#T}](./crm-quote-user-field-get.md)
- [{#T}](./crm-quote-user-field-list.md)
- [{#T}](./crm-quote-user-field-delete.md)
- [{#T}](../../universal/crm-item-update.md)
- [{#T}](./index.md)

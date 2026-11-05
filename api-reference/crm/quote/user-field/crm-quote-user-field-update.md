# Update Existing Custom Field for Quotes crm.quote.userfield.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit CRM preferences" permission in CRM

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.quote.field.custom.update](../../rest-v3/fields/crm-entity-field-custom-update-rest-v3.md).

{% endnote %}

The method `crm.quote.userfield.update` changes the parameters of an estimate custom field: labels, required flag, type settings, and list elements. The field value in a specific estimate is set by the method [crm.item.update](../../universal/crm-item-update.md) with `entityTypeId = 7`.

In REST 3.0, the field is changed by the [crm.quote.field.custom.update](../../rest-v3/fields/crm-entity-field-custom-update-rest-v3.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field.

The identifier can be obtained using the methods [crm.quote.userfield.add](./crm-quote-user-field-add.md) and [crm.quote.userfield.list](./crm-quote-user-field-list.md) ||
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

Pass in `fields` only the parameters that need to be changed.

The type `USER_TYPE_ID`, the code `FIELD_NAME`, and the multiple flag `MULTIPLE` cannot be changed. The method skips these parameters without an error and returns `true`. To get a field with a different type, code, or multiple flag, create a new field with the method [crm.quote.userfield.add](./crm-quote-user-field-add.md) ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **MANDATORY**
[`char`](../../../data-types.md) | Is the field mandatory? Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Should the field be shown in the filter? Possible values:
- `Y` — yes
- `N` — no ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional field parameters. The set of settings depends on the field type `USER_TYPE_ID`, described [below](#settings).

The method changes only the passed settings; the others remain unchanged ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the user field of type `enumeration`, description [below](#uf_enum_element) ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. Must be greater than zero ||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Should the field be shown in the list?

The parameter is not used in CRM.

Possible values:
- `Y` — yes
- `N` — no ||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allow user editing? Possible values:
- `Y` — yes
- `N` — no

The value `N` is not supported by all field types within `crm` ||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Are the field values searchable?

The parameter is not used in CRM.

Possible values:
- `Y` — yes
- `N` — no ||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Filter label in the list.

The new value completely replaces the old one:
- a string — one label for all languages
- `lang_map` — labels in the provided languages, and `''` in the other languages

To change a label in one language without erasing the others, also pass the labels in the other languages in `lang_map` ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Header in the list.

The new value completely replaces the old one:
- a string — one label for all languages
- `lang_map` — labels in the provided languages, and `''` in the other languages

To change a label in one language without erasing the others, also pass the labels in the other languages in `lang_map` ||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Label in the edit form.

The new value completely replaces the old one:
- a string — one label for all languages
- `lang_map` — labels in the provided languages, and `''` in the other languages

To change a label in one language without erasing the others, also pass the labels in the other languages in `lang_map` ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Error message. A string and a `lang_map` are processed the same way as for `LIST_FILTER_LABEL` ||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Help. A string and a `lang_map` are processed the same way as for `LIST_FILTER_LABEL` ||
|#

### Parameter SETTINGS {#settings}

The set of settings depends on the field type. The method saves only the settings from the tabs below.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value ||
    || **ROWS**
    [`integer`](../../../data-types.md) | Number of lines in the input field. Must be greater than zero ||
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
    [`integer`](../../../data-types.md) | Default value: `1` is yes, `0` is no. The method saves a number of `1` or greater as `1`, and a number of `0` or less as `0` ||
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
      - `FIXED` — use time/date from `VALUE` ||
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
    - `DIALOG` — item selection dialog ||
    || **LIST_HEIGHT**
    [`integer`](../../../data-types.md) | List height. Must be greater than zero.

    Available only when `DISPLAY = LIST` or `DISPLAY = UI` ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **IBLOCK_TYPE_ID**
    [`string`](../../../data-types.md) | Information block type identifier ||
    || **IBLOCK_ID**
    [`string`](../../../data-types.md) | Information block identifier ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance. Possible values:
    - `DIALOG` — dialog
    - `UI` — editable list
    - `LIST` — list
    - `CHECKBOX` — checkboxes ||
    || **LIST_HEIGHT**
    [`integer`](../../../data-types.md) | List height. Must be greater than zero ||
    || **ACTIVE_FILTER**
    [`char`](../../../data-types.md) | Whether to show items with the activity flag enabled. Possible values:
    - `Y` — yes
    - `N` — no ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
    || **ENTITY_TYPE**
    [`string`](../../../data-types.md) | Directory type identifier.

    Use [`crm.status.entity.types`](../../status/crm-status-entity-types.md) to find possible values ||
    |#

- crm

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`char`](../../../data-types.md) | Whether binding to [leads](../../leads/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no ||
    || **CONTACT**
    [`char`](../../../data-types.md) | Whether binding to [contacts](../../contacts/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no ||
    || **COMPANY**
    [`char`](../../../data-types.md) | Whether binding to [companies](../../companies/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no ||
    || **DEAL**
    [`char`](../../../data-types.md) | Whether binding to [deals](../../deals/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no ||
    |#

{% endlist %}

### Parameter LIST {#uf_enum_element}

{% note warning %}

The method first saves the field parameters and then the `LIST` elements. If the elements are not saved, for example, because of a duplicate `XML_ID`, the method returns an error, but the other field changes are already saved. Check the field with the method [crm.quote.userfield.get](./crm-quote-user-field-get.md) before calling the method again.

{% endnote %}

Pass only the elements that need to be added, changed, or deleted. Elements that are not in `LIST` remain unchanged. To change or delete an existing element, pass the element `ID` from the [crm.quote.userfield.get](./crm-quote-user-field-get.md) response. An element without `ID` is added as a new one.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of an existing list element ||
|| **VALUE**
[`string`](../../../data-types.md) | Value of the list element.

If you pass an empty `VALUE` together with `ID`, the element is deleted ||
|| **DEL**
[`char`](../../../data-types.md) | To delete the element with the passed `ID`, pass `Y` ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. If not passed or less than one, a new element gets `500` ||
|| **DEF**
[`char`](../../../data-types.md) | Is the list element the default value? Possible values:
- `Y` — yes
- `N` — no

A multiple field can have several default values. For a field with `MULTIPLE = N`, the first passed element with `DEF = Y` becomes the default value ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code of the value. Must be unique within the elements of the user field list ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

### Example of Changing a String Type User Field

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"N","SETTINGS":{"DEFAULT_VALUE":"Hello, World! Default value (changed)","ROWS":10},"SORT":2000,"EDIT_IN_LIST":"N","LIST_FILTER_LABEL":"Hello, World! Filter (changed)","LIST_COLUMN_LABEL":{"en":"Hello, World! Column (changed)","de":"Hallo, Welt! Spalte (geändert)"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit (changed)","de":"Hallo, Welt! Bearbeiten (geändert)"},"ERROR_MESSAGE":{"en":"Hello, World! Error (changed)","de":"Hallo, Welt! Fehler (geändert)"},"HELP_MESSAGE":{"en":"Hello, World! Help (changed)","de":"Hallo, Welt! Hilfe (geändert)"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"N","SETTINGS":{"DEFAULT_VALUE":"Hello, World! Default value (changed)","ROWS":10},"SORT":2000,"EDIT_IN_LIST":"N","LIST_FILTER_LABEL":"Hello, World! Filter (changed)","LIST_COLUMN_LABEL":{"en":"Hello, World! Column (changed)","de":"Hallo, Welt! Spalte (geändert)"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit (changed)","de":"Hallo, Welt! Bearbeiten (geändert)"},"ERROR_MESSAGE":{"en":"Hello, World! Error (changed)","de":"Hallo, Welt! Fehler (geändert)"},"HELP_MESSAGE":{"en":"Hello, World! Help (changed)","de":"Hallo, Welt! Hilfe (geändert)"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.userfield.update
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
        method: 'crm.quote.userfield.update',
        params: {
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
        console.info('User field updated successfully:', result)
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
      async function updateQuoteUserField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.userfield.update',
            params: {
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
          console.info('User field updated successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateQuoteUserField)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.quote.userfield.update(
            bitrix_id=536,
            fields={
                "MANDATORY": "N",
                "SHOW_FILTER": "N",
                "SETTINGS": {
                    "DEFAULT_VALUE": "Hello, world! Default value (changed)",
                    "ROWS": 10,
                },
                "SORT": 2000,
                "EDIT_IN_LIST": "N",
                "LIST_FILTER_LABEL": "Hello, world! Filter (changed)",
                "LIST_COLUMN_LABEL": {
                    "en": "Hello, World! Column (changed)",
                    "de": "Hallo, Welt! Spalte (geändert)",
                },
                "EDIT_FORM_LABEL": {
                    "en": "Hello, World! Edit (changed)",
                    "de": "Hallo, Welt! Bearbeiten (geändert)",
                },
                "ERROR_MESSAGE": {
                    "en": "Hello, World! Error (changed)",
                    "de": "Hallo, Welt! Fehler (geändert)",
                },
                "HELP_MESSAGE": {
                    "en": "Hello, World! Help (changed)",
                    "de": "Hallo, Welt! Hilfe (geändert)",
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
        $result = $serviceBuilder
            ->getCRMScope()
            ->quoteUserfield()
            ->update(
                536,
                [
                    'MANDATORY' => 'N',
                    'SHOW_FILTER' => 'N',
                    'SETTINGS' => [
                        'DEFAULT_VALUE' => 'Hello, world! Default value (changed)',
                        'ROWS' => 10,
                    ],
                    'SORT' => 2000,
                    'EDIT_IN_LIST' => 'N',
                    'LIST_FILTER_LABEL' => 'Hello, world! Filter (changed)',
                    'LIST_COLUMN_LABEL' => [
                        'en' => 'Hello, World! Column (changed)',
                        'de' => 'Hallo, Welt! Spalte (geändert)',
                    ],
                    'EDIT_FORM_LABEL' => [
                        'en' => 'Hello, World! Edit (changed)',
                        'de' => 'Hallo, Welt! Bearbeiten (geändert)',
                    ],
                    'ERROR_MESSAGE' => [
                        'en' => 'Hello, World! Error (changed)',
                        'de' => 'Hallo, Welt! Fehler (geändert)',
                    ],
                    'HELP_MESSAGE' => [
                        'en' => 'Hello, World! Help (changed)',
                        'de' => 'Hallo, Welt! Hilfe (geändert)',
                    ],
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
        'crm.quote.userfield.update',
        {
            id: 536,
            fields: {
                MANDATORY: 'N',
                SHOW_FILTER: 'N',
                SETTINGS: {
                    DEFAULT_VALUE: 'Hello, world! Default value (changed)',
                    ROWS: 10,
                },
                SORT: 2000,
                EDIT_IN_LIST: 'N',
                LIST_FILTER_LABEL: 'Hello, world! Filter (changed)',
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
        'crm.quote.userfield.update',
        [
            'id' => 536,
            'fields' => [
                'MANDATORY' => 'N',
                'SHOW_FILTER' => 'N',
                'SETTINGS' => [
                    'DEFAULT_VALUE' => 'Hello, world! Default value (changed)',
                    'ROWS' => 10,
                ],
                'SORT' => 2000,
                'EDIT_IN_LIST' => 'N',
                'LIST_FILTER_LABEL' => 'Hello, world! Filter (changed)',
                'LIST_COLUMN_LABEL' => [
                    'en' => 'Hello, World! Column (changed)',
                    'de' => 'Hallo, Welt! Spalte (geändert)',
                ],
                'EDIT_FORM_LABEL' => [
                    'en' => 'Hello, World! Edit (changed)',
                    'de' => 'Hallo, Welt! Bearbeiten (geändert)',
                ],
                'ERROR_MESSAGE' => [
                    'en' => 'Hello, World! Error (changed)',
                    'de' => 'Hallo, Welt! Fehler (geändert)',
                ],
                'HELP_MESSAGE' => [
                    'en' => 'Hello, World! Help (changed)',
                    'de' => 'Hallo, Welt! Hilfe (geändert)',
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
    res, err := client.Core().Call(ctx, "crm.quote.userfield.update", b24.Params{
    	"id": 536,
    	"fields": b24.Params{
    		"MANDATORY":   "N",
    		"SHOW_FILTER": "N",
    		"SETTINGS": b24.Params{
    			"DEFAULT_VALUE": "Hello, World! Default value (changed)",
    			"ROWS":          10,
    		},
    		"SORT":              2000,
    		"EDIT_IN_LIST":      "N",
    		"LIST_FILTER_LABEL": "Hello, World! Filter (changed)",
    		"LIST_COLUMN_LABEL": b24.Params{
    			"en": "Hello, World! Column (changed)",
    			"de": "Hallo, Welt! Spalte (geändert)",
    		},
    		"EDIT_FORM_LABEL": b24.Params{
    			"en": "Hello, World! Edit (changed)",
    			"de": "Hallo, Welt! Bearbeiten (geändert)",
    		},
    		"ERROR_MESSAGE": b24.Params{
    			"en": "Hello, World! Error (changed)",
    			"de": "Hallo, Welt! Fehler (geändert)",
    		},
    		"HELP_MESSAGE": b24.Params{
    			"en": "Hello, World! Help (changed)",
    			"de": "Hallo, Welt! Hilfe (geändert)",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.quote.userfield.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1753790234.592207,
        "finish": 1753790234.762644,
        "duration": 0.17043709754943848,
        "processing": 0.11566615104675293,
        "date_start": "2025-07-29T14:57:14+03:00",
        "date_finish": "2025-07-29T14:57:14+03:00",
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

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Parameter 'fields' must be array.` | The provided `fields` is not an object ||
|| `400` | Empty value | `ID is not defined or invalid.` | `id` is not passed or is less than one ||
|| `400` | Empty value | `Access denied.` | Occurs when:
- the user does not have the "Edit CRM preferences" permission in CRM
- the field with the passed `id` belongs to another object, for example, leads ||
|| `400` | `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found.` | There is no user field with the passed `id` ||
|| `400` | `ERROR_CORE` | `List element with XML_ID=xml_id already exists.` | Bitrix24 could not save the field or the list elements, for example, the `XML_ID` of an element is already used by another element of this field ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-user-field-add.md)
- [{#T}](./crm-quote-user-field-get.md)
- [{#T}](./crm-quote-user-field-list.md)
- [{#T}](./crm-quote-user-field-delete.md)
- [{#T}](../../universal/crm-item-update.md)
- [{#T}](./index.md)

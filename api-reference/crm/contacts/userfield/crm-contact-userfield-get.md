# Get Custom Contact Field by Id crm.contact.userfield.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.contact.userfield.get` returns a custom contact field by its identifier.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`][1] | Identifier of the custom field associated with the contact.

The identifier can be obtained using the methods [`crm.contact.userfield.add`](./crm-contact-userfield-add.md) or [`crm.contact.userfield.list`](./crm-contact-userfield-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Get the custom field with `id = 399`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.userfield.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.userfield.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CrmContactUserfield = {
      ID: string
      ENTITY_ID: string
      FIELD_NAME: string
      USER_TYPE_ID: string
      XML_ID: string | null
      SORT: string
      MULTIPLE: string
      MANDATORY: string
    }

    try {
      const response = await $b24.actions.v2.call.make<CrmContactUserfield>({
        method: 'crm.contact.userfield.get',
        params: {
          id: 399,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Userfield:', result.ID, result.FIELD_NAME)
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
      async function getContactUserfield() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.contact.userfield.get',
            params: {
              id: 399,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Userfield:', result.ID, result.FIELD_NAME)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getContactUserfield)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.contact.userfield.get',
                [
                    'id' => 399,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting contact user field: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.contact.userfield.get(bitrix_id=399).response
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

- BX24.js

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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.userfield.get',
        [
            'id' => 399
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
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
            "DEFAULT_VALUE": "Hello, world! Default value"
        },
        "EDIT_FORM_LABEL": {
            "ar": "Field 'Hello, world!'",
            "br": "Field 'Hello, world!'",
            "en": "Hello, World! Edit",
            "fr": "Field 'Hello, world!'",
            "id": "Field 'Hello, world!'",
            "it": "Field 'Hello, world!'",
            "ja": "Field 'Hello, world!'",
            "la": "Field 'Hello, world!'",
            "ms": "Field 'Hello, world!'",
            "pl": "Field 'Hello, world!'",
            "de": "Hello, world! Edit",
            "sc": "Field 'Hello, world!'",
            "tc": "Field 'Hello, world!'",
            "th": "Field 'Hello, world!'",
            "tr": "Field 'Hello, world!'",
            "vn": "Field 'Hello, world!'"
        },
        "LIST_COLUMN_LABEL": {
            "ar": "Field 'Hello, world!'",
            "br": "Field 'Hello, world!'",
            "en": "Hello, World! Column",
            "fr": "Field 'Hello, world!'",
            "id": "Field 'Hello, world!'",
            "it": "Field 'Hello, world!'",
            "ja": "Field 'Hello, world!'",
            "la": "Field 'Hello, world!'",
            "ms": "Field 'Hello, world!'",
            "pl": "Field 'Hello, world!'",
            "de": "Hello, world! Column",
            "sc": "Field 'Hello, world!'",
            "tc": "Field 'Hello, world!'",
            "th": "Field 'Hello, world!'",
            "tr": "Field 'Hello, world!'",
            "vn": "Field 'Hello, world!'"
        },
        "LIST_FILTER_LABEL": {
            "ar": "Hello, world! Filter",
            "br": "Hello, world! Filter",
            "en": "Hello, world! Filter",
            "fr": "Hello, world! Filter",
            "id": "Hello, world! Filter",
            "it": "Hello, world! Filter",
            "ja": "Hello, world! Filter",
            "la": "Hello, world! Filter",
            "ms": "Hello, world! Filter",
            "pl": "Hello, world! Filter",
            "de": "Hello, world! Filter",
            "sc": "Hello, world! Filter",
            "tc": "Hello, world! Filter",
            "th": "Hello, world! Filter",
            "tr": "Hello, world! Filter",
            "vn": "Hello, world! Filter"
        },
        "ERROR_MESSAGE": {
            "ar": "Field 'Hello, world!'",
            "br": "Field 'Hello, world!'",
            "en": "Hello, World! Error",
            "fr": "Field 'Hello, world!'",
            "id": "Field 'Hello, world!'",
            "it": "Field 'Hello, world!'",
            "ja": "Field 'Hello, world!'",
            "la": "Field 'Hello, world!'",
            "ms": "Field 'Hello, world!'",
            "pl": "Field 'Hello, world!'",
            "de": "Hello, world! Error",
            "sc": "Field 'Hello, world!'",
            "tc": "Field 'Hello, world!'",
            "th": "Field 'Hello, world!'",
            "tr": "Field 'Hello, world!'",
            "vn": "Field 'Hello, world!'"
        },
        "HELP_MESSAGE": {
            "ar": "Field 'Hello, world!'",
            "br": "Field 'Hello, world!'",
            "en": "Hello, World! Help",
            "fr": "Field 'Hello, world!'",
            "id": "Field 'Hello, world!'",
            "it": "Field 'Hello, world!'",
            "ja": "Field 'Hello, world!'",
            "la": "Field 'Hello, world!'",
            "ms": "Field 'Hello, world!'",
            "pl": "Field 'Hello, world!'",
            "de": "Hello, world! Help",
            "sc": "Field 'Hello, world!'",
            "tc": "Field 'Hello, world!'",
            "th": "Field 'Hello, world!'",
            "tr": "Field 'Hello, world!'",
            "vn": "Field 'Hello, world!'"
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

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`userfield`](#userfield) | Root element of the response, contains information about the custom field ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Userfield

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`][1] | Identifier of the custom field ||
|| **ENTITY_ID**
[`string`][1] | String identifier binding the custom field to the entity. 

In the case of methods `crm.contact.userfield.*`, the value `CRM_CONTACT` is automatically assigned ||
|| **FIELD_NAME**
[`string`][1] | Field code. Unique ||
|| **USER_TYPE_ID**
[`string`][1] | Data type of the custom field. Possible values:
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
- `employee` — employee binding
- `crm_status` — binding to CRM directory
- `iblock_section` — binding to information block sections
- `iblock_element` — binding to information block elements
- `crm` — binding to CRM elements
- [custom field types](../../universal/user-defined-fields/userfield-type.md)
||
|| **XML_ID**
[`string`][1] | External code ||
|| **SORT**
[`integer`][1] | Sorting index ||
|| **MULTIPLE**
[`boolean`][1] | Indicates whether the field is multiple. Possible values:
- `Y` — yes
- `N` — no
||
|| **MANDATORY**
[`boolean`][1] | Is the field mandatory? Possible values:
- `Y` — yes
- `N` — no
||
|| **SHOW_FILTER**
[`boolean`][1] | Indicates whether to show the field in the filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring
||
|| **SHOW_IN_LIST**
[`boolean`][1] | Should the user field be shown in the list?

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_IN_LIST**
[`boolean`][1] | Allows user editing. Possible values:
- `Y` — yes
- `N` — no
||
|| **IS_SEARCHABLE**
[`boolean`][1] | Are the field values searchable?

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no
||
|| **SETTINGS**
[`object`][1] | Additional field parameters. Each field type (`USER_TYPE_ID`) has its own set of available settings, which are described [below](#settings) ||
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
[`string`][1] | `CLIENT_ID` of the application that serves this field type.

Returned when the field type is custom ||
|#

### Parameter Settings {#settings}

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
    [`double`][1] | Minimum value (0 — do not check) ||
    || **MAX_VALUE**
    [`double`][1] | Maximum value (0 — do not check) ||
    || **DEFAULT_VALUE**
    [`double`][1] | Default value ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`][1] | Is it a default value. Possible values:
    - `0` — no
    - `1` — yes ||
    || **DISPLAY**
    [`string`][1] | Appearance. Possible values:
    - `CHECKBOX` — checkbox
    - `RADIO` — radio buttons
    - `DROPDOWN` — dropdown list ||
    || **LABEL**
    [`string[]`][1] | Labels for values, where:
    - array item with index `0` — label for value `No`
    - array item with index `1` — label for value `Yes`
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

    where:
    - `TYPE` — default value type:
        - `NONE` — none
        - `NOW` — current date
        - `FIXED` — date from `VALUE`
    - `VALUE` has type `date` ||
    |#

- integer

    #|
    || **Name**
    `type` | **Description** ||
    || **SIZE**
    [`integer`][1] | Input field size for display ||
    || **MIN_VALUE**
    [`integer`][1] | Minimum value (0 — do not check) ||
    || **MAX_VALUE**
    [`integer`][1] | Maximum value (0 — do not check) ||
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

    where:
    - `TYPE` — default value type:
        - `NONE` — none
        - `NOW` — current date and time
        - `FIXED` — date and time from `VALUE`
    - `VALUE` has type `datetime` ||
    || **USE_SECOND**
    [`boolean`][1] | Use seconds. Possible values:
    - `Y` — yes
    - `N` — no ||
    || **USE_TIMEZONE**
    [`boolean`][1] | Use time zones. Possible values:
    - `Y` — yes
    - `N` — no ||
    |#

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **SIZE**
    [`integer`][1] | Input field size for display ||
    || **ROWS**
    [`integer`][1] | Number of input field lines ||
    || **REGEXP**
    [`string`][1] | Regular expression for validation ||
    || **MIN_LENGTH**
    [`integer`][1] | Minimum string length (0 — do not check) ||
    || **MAX_LENGTH**
    [`integer`][1] | Maximum string length (0 — do not check) ||
    || **DEFAULT_VALUE**
    [`string`][1] | Default value ||
    |#

- enumeration

    #|
    || **Name**
    `type` | **Description** ||
    || **DISPLAY**
    [`string`][1] | Appearance. Possible values:
    - `LIST` — list
    - `CHECKBOX` — checkboxes
    - `UI` — searchable list
    - `DIALOG` — entity selection dialog ||
    || **LIST_HEIGHT**
    [`integer`][1] | List height ||
    || **CAPTION_NO_VALUE**
    [`string`][1] | Label when value is missing ||
    || **SHOW_NO_VALUE**
    [`boolean`][1] | Whether to show an empty value for a required field. Possible values:
    - `Y` — yes
    - `N` — no ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **DISPLAY**
    [`string`][1] | Appearance. Possible values:
    - `LIST` — list
    - `CHECKBOX` — checkboxes
    - `UI` — searchable list
    - `DIALOG` — entity selection dialog ||
    || **LIST_HEIGHT**
    [`integer`][1] | List height ||
    || **IBLOCK_ID**
    [`integer`][1] | Information block ID ||
    || **DEFAULT_VALUE**
    [`integer`][1] | Default value ||
    || **ACTIVE_FILTER**
    [`boolean`][1] | Show only active items. Possible values:
    - `Y` — yes
    - `N` — no ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
    || **ENTITY_TYPE**
    [`object`][1] | CRM directory. The structure is similar to the elements returned by the [`crm.status.entity.types`](../../status/crm-status-entity-types.md) method ||
    |#

- crm

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`boolean`][1] | Whether binding to Leads is enabled ||
    || **CONTACT**
    [`boolean`][1] | Whether binding to Contacts is enabled ||
    || **COMPANY**
    [`boolean`][1] | Whether binding to Companies is enabled ||
    || **DEAL**
    [`boolean`][1] | Whether binding to Deals is enabled ||
    || **ORDER**
    [`boolean`][1] | Whether binding to Orders is enabled ||
    || **QUOTE**
    [`boolean`][1] | Whether binding to Commercial proposals is enabled ||
    || **SMART_INVOICE**
    [`boolean`][1] | Whether binding to New invoices is enabled ||
    || **DYNAMIC_...**
    [`boolean`][1] | Whether binding to a specific SPA is enabled.

    Each such field has the form: `DYNAMIC_{entityTypeId}`, where `entityTypeId` is the SPA type ID to which the binding is enabled ||
    |#

- money

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`][1] | Default value.

    The value of this field has the format: `{VALUE}\|{CURRENCY}`, where:
    - `VALUE` — default amount of money
    - `CURRENCY` — string currency identifier

    For example: `300\|USD` — 300 dollars

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
    [`integer`][1] | Minimum string length (0 — do not check) ||
    || **MAX_LENGTH**
    [`integer`][1] | Maximum string length (0 — do not check) ||
    || **DEFAULT_VALUE**
    [`string`][1] | Default value ||
    || **ROWS**
    [`integer`][1] | Number of input field lines ||
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
    [`integer`][1] | Maximum allowable size for display in the list (0 — no limit) ||
    || **MAX_ALLOWED_SIZE**
    [`integer`][1] | Maximum allowable file size for upload (0 — do not check) ||
    || **EXTENSIONS**
    [`string[]`][1] | Allowed extensions ||
    || **TARGET_BLANK**
    [`boolean`][1] | Open file in a new tab ||
    |#

{% endlist %}

### uf_enum_element Type {#uf_enum_element}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`][1] | Identifier of the list element ||
|| **VALUE**
[`string`][1] | Value of the list element ||
|| **SORT**
[`integer`][1] | Sorting index ||
|| **DEF**
[`boolean`][1] | Indicates whether the list element is the default value. Possible values:
- `Y` — yes
- `N` — no
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

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `Access denied` | Occurs when:
- the user does not have administrative rights
- the user attempts to access a custom field not linked to contacts ||
|| `-` | `ID is not defined or invalid` | The provided `id` is less than or equal to zero, or not provided at all ||
|| `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found` | The custom field with the provided `id` was not found ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-userfield-add.md)
- [{#T}](./crm-contact-userfield-update.md)
- [{#T}](./crm-contact-userfield-list.md)
- [{#T}](./crm-contact-userfield-delete.md)

[1]: ../../../data-types.md
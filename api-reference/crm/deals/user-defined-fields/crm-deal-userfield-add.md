# Create a Custom Field for Deals crm.deal.userfield.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator CRM

The method `crm.deal.userfield.add` creates a new custom field for deals.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

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

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_TYPE_ID***
[`string`](../../../data-types.md) | Data type of the custom field. Possible values:
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
[`string`](../../../data-types.md) | Field code. Unique.

The system limit for the field code is 20 characters. The custom field name always has the prefix `UF_CRM_`, meaning the actual name length is 13 characters.

Allowed characters: `A-Z`, `0-9`, and `_`||
|| **LABEL**
[`string`](../../../data-types.md) | Default name of the custom field.

The provided value will be set in the following fields: `LIST_FILTER_LABEL`, `LIST_COLUMN_LABEL`, `EDIT_FORM_LABEL`, `ERROR_MESSAGE`, `HELP_MESSAGE`, if no value is provided for them ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code ||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Filter label in the list.

When a string is provided, it will be set for all language identifiers.

When a `lang_map` type value is provided, the value from `LABEL` will be set for all languages that were not provided.

By default, the value passed in `LABEL` is set for all language identifiers ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Header in the list.

When a string is provided, it will be set for all language identifiers.

When a `lang_map` type value is provided, the value from `LABEL` will be set for all languages that were not provided.

By default, the value passed in `LABEL` is set for all language identifiers ||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Label in the edit form.

When a string is provided, it will be set for all language identifiers.

When a `lang_map` type value is provided, the value from `LABEL` will be set for all languages that were not provided.

By default, the value passed in `LABEL` is set for all language identifiers ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Error message||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md#lang-ids) | Help||
|| **MULTIPLE**
[`boolean`](../../../data-types.md) | Is the field multiple. Possible values:
- `Y` — yes
- `N` — no

Fields of type `boolean` cannot be multiple.

By default, `N` ||
|| **MANDATORY**
[`boolean`](../../../data-types.md) | Is the field mandatory. Possible values:
- `Y` — yes
- `N` — no

By default, `N` ||
|| **SHOW_FILTER**
[`boolean`](../../../data-types.md) | Show the field in the filter. Possible values:
- `Y` — yes
- `N` — no

By default, `N` ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional field parameters. Each `USER_TYPE_ID` field type has its own set of available settings, described [below](#settings) ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the custom field of type `enumeration`, described [below](#uf_enum_element)

By default `[]` ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. Must be greater than zero.

By default `100` ||
|| **SHOW_IN_LIST**
[`boolean`](../../../data-types.md) | Show the custom field in the list.

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no

By default `N` ||
|| **EDIT_IN_LIST**
[`boolean`](../../../data-types.md) | Allow user editing. Possible values:
- `Y` — yes
- `N` — no

By default `Y`. The value `N` is not supported by all field types within `crm` ||
|| **IS_SEARCHABLE**
[`boolean`](../../../data-types.md) | Are the field values included in the search.

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no

By default `N` ||
|#

### Parameter SETTINGS {#settings}

Each type of custom fields has its own set of additional settings. This method only supports those described below.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value.

    Default `''` ||
    || **ROWS**
    [`integer`](../../../data-types.md) | Number of lines in the input field. Must be greater than 0.

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

    Default `2` ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../../data-types.md) | Default value, where `1` is yes, `0` is no.

    Possible values:
    - `>= 1` -> 1
    - `<= 0` -> 0

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
    - `DIALOG` — entity selection dialog

    Default `LIST` ||
    || **LIST_HEIGHT** | List height. Must be greater than 0.

    Available only when `DISPLAY = LIST` or `DISPLAY = UI`.

    Default `1` ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **IBLOCK_TYPE_ID**
    [`string`](../../../data-types.md) | Infoblock type identifier.

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
    [`integer`](../../../data-types.md) | List height. Must be greater than 0.

    Default `1` ||
    || **ACTIVE_FILTER**
    [`boolean`](../../../data-types.md) | Whether to show elements with the activity flag enabled. Possible values:
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

    If none of the following options are provided, the link to leads will be enabled by default `LEAD = Y`.

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`boolean`](../../../data-types.md) | Whether binding to [Leads](../../leads/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default `N` ||
    || **CONTACT**
    [`boolean`](../../../data-types.md) | Whether binding to [Contacts](../index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default `N` ||
    || **COMPANY**
    [`boolean`](../../../data-types.md) | Whether binding to [Companies](../../companies/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default `N` ||
    || **DEAL**
    [`boolean`](../../../data-types.md) | Whether binding to [Deals](../index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default `N` ||
    |#

{% endlist %}

### Parameter LIST {#uf_enum_element}

#|
|| **Name**
`type` | **Description** ||
|| **VALUE**
[`string`](../../../data-types.md) | Value of the list item.

List items with an empty or missing `VALUE` will be ignored ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. Must be greater than or equal to 0.

By default `0` ||
|| **DEF**
[`boolean`](../../../data-types.md) | Is the list item the default value. Possible values:
- `Y` — yes
- `N` — no

For a multiple field, multiple `DEF = Y` are allowed. For a non-multiple field, the first provided list item with `DEF = Y` will be considered default.

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
    -d '{"fields":{"LABEL":"Field 'Hello, World!'","USER_TYPE_ID":"string","FIELD_NAME":"HELLO_WORLD","MULTIPLE":"Y","MANDATORY":"Y","SHOW_FILTER":"Y","SETTINGS":{"DEFAULT_VALUE":"Hello, World! Default value","ROWS":3},"SORT":1000,"EDIT_IN_LIST":"Y","LIST_FILTER_LABEL":"Hello, World! Filter","LIST_COLUMN_LABEL":{"en":"Hello, World! Column","de":"Hallo, Welt! Spalte"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit","de":"Hallo, Welt! Bearbeiten"},"ERROR_MESSAGE":{"en":"Hello, World! Error","de":"Hallo, Welt! Fehler"},"HELP_MESSAGE":{"en":"Hello, World! Help","de":"Hallo, Welt! Hilfe"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.userfield.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"LABEL":"Field 'Hello, World!'","USER_TYPE_ID":"string","FIELD_NAME":"HELLO_WORLD","MULTIPLE":"Y","MANDATORY":"Y","SHOW_FILTER":"Y","SETTINGS":{"DEFAULT_VALUE":"Hello, World! Default value","ROWS":3},"SORT":1000,"EDIT_IN_LIST":"Y","LIST_FILTER_LABEL":"Hello, World! Filter","LIST_COLUMN_LABEL":{"en":"Hello, World! Column","de":"Hallo, Welt! Spalte"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit","de":"Hallo, Welt! Bearbeiten"},"ERROR_MESSAGE":{"en":"Hello, World! Error","de":"Hallo, Welt! Fehler"},"HELP_MESSAGE":{"en":"Hello, World! Help","de":"Hallo, Welt! Hilfe"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.userfield.add
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.deal.userfield.add',
        {
            fields: {
                LABEL: "Field 'Hello, world!'",
                USER_TYPE_ID: "string",
                FIELD_NAME: "HELLO_WORLD",
                MULTIPLE: "Y",
                MANDATORY: "Y",
                SHOW_FILTER: "Y",
                SETTINGS: {
                    DEFAULT_VALUE: "Hello, world! Default value",
                    ROWS: 3,
                },
                SORT: 1000,
                EDIT_IN_LIST: "Y",
                LIST_FILTER_LABEL: "Hello, world! Filter",
                LIST_COLUMN_LABEL: {
                    "en": "Hello, World! Column",
                    "de": "Hallo, Welt! Spalte"
                },
                EDIT_FORM_LABEL: {
                    "en": "Hello, World! Edit",
                    "de": "Hallo, Welt! Bearbeiten"
                },
                ERROR_MESSAGE: {
                    "en": "Hello, World! Error",
                    "de": "Hallo, Welt! Fehler"
                },
                HELP_MESSAGE: {
                    "en": "Hello, World! Help",
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
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.userfield.add',
        [
            'fields' => [
                'LABEL' => "Field 'Hello, world!'",
                'USER_TYPE_ID' => "string",
                'FIELD_NAME' => "HELLO_WORLD",
                'MULTIPLE' => "Y",
                'MANDATORY' => "Y",
                'SHOW_FILTER' => "Y",
                'SETTINGS' => [
                    'DEFAULT_VALUE' => "Hello, world! Default value",
                    'ROWS' => 3,
                ],
                'SORT' => 1000,
                'EDIT_IN_LIST' => "Y",
                'LIST_FILTER_LABEL' => "Hello, world! Filter",
                'LIST_COLUMN_LABEL' => [
                    'en' => "Hello, World! Column",
                    'de' => "Hallo, Welt! Spalte"
                ],
                'EDIT_FORM_LABEL' => [
                    'en' => "Hello, World! Edit",
                    'de' => "Hallo, Welt! Bearbeiten"
                ],
                'ERROR_MESSAGE' => [
                    'en' => "Hello, World! Error",
                    'de' => "Hallo, Welt! Fehler"
                ],
                'HELP_MESSAGE' => [
                    'en' => "Hello, World! Help",
                    'de' => "Hallo, Welt! Hilfe"
                ],
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.deal.userfield.add(
            fields={
                "LABEL": "Field 'Hello, world!'",
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
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP (B24PhpSdk)

    ```php
    try {
        $userfieldItemFields = [
            'FIELD_NAME' => 'Test Field',
            'USER_TYPE_ID' => 'string',
            'XML_ID' => 'test_field_1',
            'SORT' => '100',
            'MULTIPLE' => 'N',
            'MANDATORY' => 'N',
            'SHOW_FILTER' => 'Y',
            'SHOW_IN_LIST' => 'Y',
            'EDIT_IN_LIST' => 'Y',
            'IS_SEARCHABLE' => 'Y',
            'EDIT_FORM_LABEL' => 'Test Field Label',
            'LIST_COLUMN_LABEL' => 'Test Field List Label',
            'LIST_FILTER_LABEL' => 'Test Field Filter Label',
            'ERROR_MESSAGE' => 'Error occurred',
            'HELP_MESSAGE' => 'Help message for Test Field',
            'LIST' => '',
            'SETTINGS' => '',
        ];

        $result = $serviceBuilder
            ->getCRMScope()
            ->dealUserfield()
            ->add($userfieldItemFields);

        print($result->getId());
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.deal.userfield.add(
            fields={
                "FIELD_NAME": "UF_CRM_DEAL_COMMENT_TEXT",
                "USER_TYPE_ID": "string",
                "XML_ID": "deal_comment_text",
                "SORT": 100,
                "MULTIPLE": "N",
                "MANDATORY": "N",
                "SHOW_FILTER": "Y",
                "EDIT_FORM_LABEL": {"de": "Deal comment"},
                "LIST_FILTER_LABEL": {"de": "Deal comment"},
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

{% endlist %}

### Example of Creating a Custom Field of Type List

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"LABEL":"Custom field (list)","USER_TYPE_ID":"enumeration","FIELD_NAME":"ENUMERATION_EXAMPLE","MULTIPLE":"N","MANDATORY":"N","SHOW_FILTER":"Y","LIST":[{"VALUE":"List item #1","DEF":"Y","XML_ID":"XML_ID_1","SORT":100},{"VALUE":"List item #2","XML_ID":"XML_ID_2","SORT":200},{"VALUE":"List item #3","XML_ID":"XML_ID_3","SORT":300},{"VALUE":"List item #4","XML_ID":"XML_ID_4","SORT":400}],"SETTINGS":{"DISPLAY":"UI","LIST_HEIGHT":2},"SORT":2000}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.userfield.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"LABEL":"Custom field (list)","USER_TYPE_ID":"enumeration","FIELD_NAME":"ENUMERATION_EXAMPLE","MULTIPLE":"N","MANDATORY":"N","SHOW_FILTER":"Y","LIST":[{"VALUE":"List item #1","DEF":"Y","XML_ID":"XML_ID_1","SORT":100},{"VALUE":"List item #2","XML_ID":"XML_ID_2","SORT":200},{"VALUE":"List item #3","XML_ID":"XML_ID_3","SORT":300},{"VALUE":"List item #4","XML_ID":"XML_ID_4","SORT":400}],"SETTINGS":{"DISPLAY":"UI","LIST_HEIGHT":2},"SORT":2000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.userfield.add
    ```

- JS

    ```js
    BX.rest.callMethod(
        'crm.deal.userfield.add',
        {
            fields: {
                LABEL: "Custom field (list)",
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
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.userfield.add',
        [
            'fields' => [
                'LABEL' => "Custom field (list)",
                'USER_TYPE_ID' => "enumeration",
                'FIELD_NAME' => "ENUMERATION_EXAMPLE",
                'MULTIPLE' => "N",
                'MANDATORY' => "N",
                'SHOW_FILTER' => "Y",
                'LIST' => [
                    [
                        'VALUE' => "List item #1",
                        'DEF' => "Y",
                        'XML_ID' => "XML_ID_1",
                        'SORT' => 100,
                    ],
                    [
                        'VALUE' => "List item #2",
                        'XML_ID' => "XML_ID_2",
                        'SORT' => 200,
                    ],
                    [
                        'VALUE' => "List item #3",
                        'XML_ID' => "XML_ID_3",
                        'SORT' => 300,
                    ],
                    [
                        'VALUE' => "List item #4",
                        'XML_ID' => "XML_ID_4",
                        'SORT' => 400,
                    ],
                ],
                'SETTINGS' => [
                    'DISPLAY' => "UI",
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

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.deal.userfield.add(
            fields={
                "LABEL": "Custom field (list)",
                "USER_TYPE_ID": "enumeration",
                "FIELD_NAME": "ENUMERATION_EXAMPLE",
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
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `The 'FIELD_NAME' field is not found` | Either an empty `FIELD_NAME` was provided, or it was not provided at all ||
|| `400` | `Field name is too long (more than 50 characters).` | The provided `FIELD_NAME` contains more than 50 characters ||
|| `400` | `Field name contains invalid characters. Allowed characters are: A-Z, 0-9, and _.` | The provided `FIELD_NAME` contains invalid characters ||
|| `400` | `The 'USER_TYPE_ID' field is not found` | Either an empty `USER_TYPE_ID` was provided, or it was not provided at all ||
|| `400` | Invalid custom type specified | The provided `USER_TYPE_ID` does not exist ||
|| `400` | `List item with value XML_ID='XML_ID' already exists` | The provided `XML_ID` in list items are not unique ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-userfield-update.md)
- [{#T}](./crm-deal-userfield-get.md)
- [{#T}](./crm-deal-userfield-list.md)
- [{#T}](./crm-deal-userfield-delete.md)

# Update Existing Custom Contact Field crm.contact.userfield.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.contact.userfield.update` updates an existing custom contact field.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`][1] | Identifier of the custom field.

The identifier can be obtained using the methods [`crm.contact.userfield.add`](./crm-contact-userfield-add.md) and [`crm.contact.userfield.list`](./crm-contact-userfield-list.md) ||
|| **fields***
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
- `field_n` — field name
- `value_n` — new field value

The list of available fields is described [below](#parameter-fields).

An incorrect field in `fields` will be ignored.

Only those fields that need to be changed should be passed in `fields` ||
|#

### Parameter fields {#parameter-fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **MANDATORY**
[`boolean`][1] | Is the field mandatory? Possible values:
- `Y` — yes
- `N` — no
||
|| **SHOW_FILTER**
[`boolean`][1] | Should the field be shown in the filter? Possible values:
- `Y` — yes
- `N` — no
||
|| **XML_ID**
[`string`][1] | External code ||
|| **SETTINGS**
[`object`][1] | Additional field parameters. Each field type (`USER_TYPE_ID`) has its own set of available settings, which are described [below](#settings).

The field only overwrites the passed values ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the custom field of type `enumeration`. For custom fields of other types, this parameter is meaningless ||
|| **SORT**
[`integer`][1] | Sort index. Must be greater than zero ||
|| **SHOW_IN_LIST**
[`boolean`][1] | Should the user field be shown in the list?

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_IN_LIST**
[`boolean`][1] | Allow user editing? Possible values:
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
|| **LIST_FILTER_LABEL**
[`string`][1]\|[`lang_map`](../../data-types.md) | Filter label in the list.

When a string is passed, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **LIST_COLUMN_LABEL**
[`string`][1]\|[`lang_map`](../../data-types.md) | Header in the list.

When a string is passed, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **EDIT_FORM_LABEL**
[`string`][1]\|[`lang_map`](../../data-types.md) | Label in the edit form.

When a string is passed, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **ERROR_MESSAGE**
[`string`][1]\|[`lang_map`](../../data-types.md) | Error message.

When passing a string, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **HELP_MESSAGE**
[`string`][1]\|[`lang_map`](../../data-types.md) | Help message.

When passing a string, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|#

### Parameter SETTINGS {#settings}

Each type of custom field has its own set of additional settings. This method supports changing only those described below.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`][1] | Default value ||
    || **ROWS**
    [`integer`][1] | Number of lines in the input field. Must be greater than 0 and less than 50.

    If a value <= 0 is passed, the value `1` will be set.

    If a value >= 50 is passed, the value `50` will be set.
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
    [`integer`][1] | Number precision. Must be greater than or equal to 0.

    If an invalid value is passed, the value `2` will be set ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`][1] | Default value, where `1` is yes, `0` is no.

    When passing a value, it will be set according to the rule:
    - `>= 1` -> 1
    - `<= 0` -> 0
    ||
    || **DISPLAY**
    [`string`][1] | Appearance. Possible values:
    - `CHECKBOX` — checkbox
    - `RADIO` — radio buttons
    - `DROPDOWN` — dropdown list
    ||
    |#

- datetime

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`object`][1]  | Default value. Object format:
    ```
    {
        VALUE: datetime,
        TYPE: 'NONE'|'NOW'|'FIXED',
    }
    ```

    where
    - `VALUE` — default value of type `datetime`
    - `TYPE` — default value type:
        - `NONE` — do not set a default value
        - `NOW` — use current time/date
        - `FIXED` — use time/date from `VALUE`

    If an invalid value is passed, the following will be set:
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
    [`string`][1] | Appearance. Possible values:
    - `LIST` — list
    - `UI` — input list
    - `CHECKBOX` — checkboxes
    - `DIALOG` — entity selection dialog
    ||
    || **LIST_HEIGHT** | List height. Must be greater than 0 ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **IBLOCK_TYPE_ID**
    [`string`][1] | Infoblock type identifier ||
    || **IBLOCK_ID**
    [`string`][1] | Infoblock identifier ||
    || **DEFAULT_VALUE**
    [`string`][1] | Default value ||
    || **DISPLAY**
    [`string`][1] | Appearance. Possible values:
    - `DIALOG` — dialog
    - `UI` — input list
    - `LIST` — list
    - `CHECKBOX` — checkboxes
    ||
    || **LIST_HEIGHT**
    [`integer`][1] | List height. Must be greater than 0
    ||
    || **ACTIVE_FILTER**
[`boolean`][1] | Whether to show items with the activity flag enabled. Possible values:
- `Y` — yes
- `N` — no
    ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
    || **ENTITY_TYPE**
[`string`][1] | Directory type identifier.

Use [`crm.status.entity.types`](../../status/crm-status-entity-types.md) to find possible values ||
    |#

- crm

    If none of the following options are passed, the binding to leads will be enabled by default (`LEAD = Y`)

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
[`boolean`][1] | Whether binding to [Leads](../../leads/index.md) is enabled. Possible values:
- `Y` — yes
- `N` — no
    ||
    || **CONTACT**
[`boolean`][1] | Whether binding to [Contacts](../index.md) is enabled. Possible values:
- `Y` — yes
- `N` — no
    ||
    || **COMPANY**
[`boolean`][1] | Whether binding to [Companies](../../companies/index.md) is enabled. Possible values:
- `Y` — yes
- `N` — no
    ||
    || **DEAL**
[`boolean`][1] | Whether binding to [Deals](../../deals/index.md) is enabled. Possible values:
- `Y` — yes
- `N` — no
    ||
    |#

{% endlist %}

### Uf_Enum_Element Type {#uf_enum_element}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`][1] | Identifier of the list item. When passing this parameter, the corresponding list item will be changed; otherwise, a new list item will be added.

The identifier can be obtained using the method [`crm.contact.userfield.get`](./crm-contact-userfield-get.md#uf_enum_element)
||
|| **DEL**
[`boolean`][1] | Flag necessary for deleting a list item. Makes sense only when passing `ID`. 

Possible values:
`Y` — delete
`N` — do not delete

Default is `N`
||
|| **VALUE**
[`string`][1] | Value of the list element ||
|| **SORT**
[`integer`][1] | Sort index. Must be greater than or equal to 0 ||
|| **DEF**
[`boolean`][1] | Is the list item the default value? Possible values:
- `Y` — yes
- `N` — no

For a multiple field, multiple `DEF = Y` is allowed. For a non-multiple field, the first passed list item with `DEF = Y` will be considered the default value ||
|| **XML_ID**
[`string`][1] | External code of the value. Must be unique within the elements of the user field list ||
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
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"N","SETTINGS":{"DEFAULT_VALUE":"Hello, world! Default value (changed)","ROWS":10},"SORT":2000,"EDIT_IN_LIST":"N","LIST_FILTER_LABEL":"Hello, world! Filter (changed)","LIST_COLUMN_LABEL":{"en":"Hello, World! Column (changed)","de":"Hello, world! Column (changed)","de":"Hallo, Welt! Spalte (geändert)"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit (changed)","de":"Hallo, Welt! Bearbeiten (geändert)"},"ERROR_MESSAGE":{"en":"Hello, World! Error (changed)","de":"Hello, world! Error (changed)","de":"Hallo, Welt! Fehler (geändert)"},"HELP_MESSAGE":{"en":"Hello, World! Help (changed)","de":"Hallo, Welt! Hilfe (geändert)"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"N","SETTINGS":{"DEFAULT_VALUE":"Hello, world! Default value (changed)","ROWS":10},"SORT":2000,"EDIT_IN_LIST":"N","LIST_FILTER_LABEL":"Hello, world! Filter (changed)","LIST_COLUMN_LABEL":{"en":"Hello, World! Column (changed)","de":"Hello, world! Column (changed)","de":"Hallo, Welt! Spalte (geändert)"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit (changed)","de":"Hallo, Welt! Bearbeiten (geändert)"},"ERROR_MESSAGE":{"en":"Hello, World! Error (changed)","de":"Hello, world! Error (changed)","de":"Hallo, Welt! Fehler (geändert)"},"HELP_MESSAGE":{"en":"Hello, World! Help (changed)","de":"Hallo, Welt! Hilfe (geändert)"}}, "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.userfield.update
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
                    DEFAULT_VALUE: "Hello, world! Default value (changed)",
                    ROWS: 10,
                },
                SORT: 2000,
                EDIT_IN_LIST: "N",
                LIST_FILTER_LABEL: "Hello, world! Filter (changed)",
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
        'crm.contact.userfield.update',
        [
            'id' => 536,
            'fields' => [
                'MANDATORY' => "N",
                'SHOW_FILTER' => "N",
                'SETTINGS' => [
                    'DEFAULT_VALUE' => "Hello, world! Default value (changed)",
                    'ROWS' => 10,
                ],
                'SORT' => 2000,
                'EDIT_IN_LIST' => "N",
                'LIST_FILTER_LABEL' => "Hello, world! Filter (changed)",
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

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.contact.userfield.update(
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

- PHP (B24PhpSdk)

    ```php
    try {
        $contactUserfieldItemId = 123; // Example ID
        $userfieldFieldsToUpdate = [
            'FIELD_NAME' => 'New Field Name',
            'USER_TYPE_ID' => 'string',
            'SORT' => '100',
            'MULTIPLE' => 'N',
            'MANDATORY' => 'N',
            'SHOW_FILTER' => 'Y',
            'SHOW_IN_LIST' => 'Y',
            'EDIT_IN_LIST' => 'Y',
            'IS_SEARCHABLE' => 'Y',
            'EDIT_FORM_LABEL' => 'New Label',
            'LIST_COLUMN_LABEL' => 'Column Label',
            'LIST_FILTER_LABEL' => 'Filter Label',
            'ERROR_MESSAGE' => 'Error Message',
            'HELP_MESSAGE' => 'Help Message',
            'LIST' => '',
            'SETTINGS' => '',
        ];

        $result = $serviceBuilder
            ->getCRMScope()
            ->contactUserfield()
            ->update($contactUserfieldItemId, $userfieldFieldsToUpdate);

        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print("Update failed.");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

{% endlist %}

### Example of Updating a List Type Custom Field

Current list items:

```json
[
    {
        "ID": "115",
        "SORT": "100",
        "VALUE": "List item #1",
        "DEF": "Y",
        "XML_ID": "XML_ID_1"
    },
    {
        "ID": "116",
        "SORT": "200",
        "VALUE": "List item #2",
        "DEF": "N",
        "XML_ID": "XML_ID_2"
    },
    {
        "ID": "117",
        "SORT": "300",
        "VALUE": "List item #3",
        "DEF": "N",
        "XML_ID": "XML_ID_3"
    },
    {
        "ID": "118",
        "SORT": "400",
        "VALUE": "List item #4",
        "DEF": "N",
        "XML_ID": "XML_ID_4"
    }
]
```

Change it as follows:
- remove list items with `ID = 115` and `ID = 116`
- update the list item with `ID  = 117`:
    - `VALUE`: “List item #3” -> “List item #3 (changed)”
    - `SORT`: 300 -> 50
- add a new list item “List item #5”

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"MANDATORY":"N","SHOW_FILTER":"Y","LIST":[{"ID":115,"DEL":"Y"},{"ID":116,"DEL":"Y"},{"ID":117,"VALUE":"List item #3 (changed)","SORT":50},{"VALUE":"List item #5","XML_ID":"XML_ID_5","SORT":500}],"SETTINGS":{"DISPLAY":"DIALOG","LIST_HEIGHT":3},"SORT":1000}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"MANDATORY":"N","SHOW_FILTER":"Y","LIST":[{"ID":115,"DEL":"Y"},{"ID":116,"DEL":"Y"},{"ID":117,"VALUE":"List item #3 (changed)","SORT":50},{"VALUE":"List item #5","XML_ID":"XML_ID_5","SORT":500}],"SETTINGS":{"DISPLAY":"DIALOG","LIST_HEIGHT":3},"SORT":1000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.userfield.update
    ```

- JS

    ```js
    BX24.callMethod(
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
                        VALUE: "List item #3 (changed)",
                        SORT: 50,
                    },
                    {
                        VALUE: "List item #5",
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
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.userfield.update',
        [
            'fields' => [
                'MANDATORY' => "N",
                'SHOW_FILTER' => "Y",
                'LIST' => [
                    [
                        'ID' => 115,
                        'DEL' => "Y"
                    ],
                    [
                        'ID' => 116,
                        'DEL' => "Y",
                    ],
                    [
                        'ID' => 117,
                        'VALUE' => "List item #3 (changed)",
                        'SORT' => 50,
                    ],
                    [
                        'VALUE' => "List item #5",
                        'XML_ID' => "XML_ID_5",
                        'SORT' => 500,
                    ],
                ],
                'SETTINGS' => [
                    'DISPLAY' => "DIALOG",
                    'LIST_HEIGHT' => 3,
                ],
                'SORT' => 1000,
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
        bitrix_response = client.crm.contact.userfield.update(
            bitrix_id=536,
            fields={
                "MANDATORY": "N",
                "SHOW_FILTER": "Y",
                "SETTINGS": {
                    "DISPLAY": "DIALOG",
                    "LIST_HEIGHT": 3,
                },
                "SORT": 1000,
            },
            list=[
                {
                    "ID": 115,
                    "DEL": "Y",
                },
                {
                    "ID": 116,
                    "DEL": "Y",
                },
                {
                    "ID": 117,
                    "VALUE": "List item #3 (changed)",
                    "SORT": 50,
                },
                {
                    "VALUE": "List item #5",
                    "XML_ID": "XML_ID_5",
                    "SORT": 500,
                },
            ],
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

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response, contains `true` in case of success ||
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
|| **Code** | **Description** | **Value** ||
|| `-`     | `Parameter 'fields' must be array` | The provided `fields` is not an object ||
|| `-`     | `ID is not defined or invalid`     | The passed `id` is less than zero or not passed at all ||
|| `-`     | `Access denied`                    | Occurs when:
- the user does not have administrative rights
- the user attempts to delete a custom field not associated with contacts ||
|| `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found` | The user field with the passed `id` does not exist ||
|| `ERROR_CORE`               | List item with XML_ID=`XML_ID` already exists | The passed `XML_ID` for the list element must be unique within the elements of the user field list ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-userfield-add.md)
- [{#T}](./crm-contact-userfield-get.md)
- [{#T}](./crm-contact-userfield-list.md)
- [{#T}](./crm-contact-userfield-delete.md)

[1]: ../../../data-types.md

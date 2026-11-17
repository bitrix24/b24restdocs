# Edit an Existing Contact Custom Field crm.contact.userfield.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit CRM preferences" permission in CRM

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.contact.field.custom.update](../../rest-v3/fields/crm-entity-field-custom-update-rest-v3.md). In REST 3.0, the [crm.contact.field.custom.enum.*](../../rest-v3/fields/crm-entity-field-custom-enum-list-rest-v3.md) methods change the values of List `enumeration` fields.

{% endnote %}

The `crm.contact.userfield.update` method updates an existing contact custom field.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field.

The identifier can be obtained using the methods [`crm.contact.userfield.add`](./crm-contact-userfield-add.md) and [`crm.contact.userfield.list`](./crm-contact-userfield-list.md) ||
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

where:
- `field_n` — field name
- `value_n` — new field value

The list of available fields is described [below](#parameter-fields).

An incorrect field in `fields` will be ignored. This method cannot change `FIELD_NAME`, `USER_TYPE_ID`, or `MULTIPLE`: provided values are not applied.

Only those fields that need to be changed should be passed in `fields` ||
|#

### Parameter fields {#parameter-fields}

#|
|| **Name**
`type` | **Description** ||
|| **MANDATORY**
[`char`](../../../data-types.md) | Is the field mandatory? Possible values:
- `Y` — yes
- `N` — no
||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Should the field be shown in the filter? Possible values:
- `Y` — yes
- `N` — no

When `Y` is passed, `E` is stored for mask filtering. For the `employee` field type, `I` is stored for exact matching. When `N` is passed, `N` is stored
||
|| **XML_ID**
[`string`](../../../data-types.md) | External code ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional field parameters. The provided settings are merged with the saved settings and then normalized by the field type handler. Available settings are described [below](#settings) ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the custom field of type `enumeration`. This parameter is not used for other field types ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index ||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Should the user field be shown in the list?

Affects the default set of contact list columns. The user can customize the columns.

Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allow user editing? Possible values:
- `Y` — yes
- `N` — no
||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Are the field values searchable?

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no
||
|| **LIST_FILTER_LABEL**
```string | lang_map``` | Filter label in the list.

When a string is passed, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **LIST_COLUMN_LABEL**
```string | lang_map``` | Header in the list. Strings and `lang_map` are processed in the same way as for `LIST_FILTER_LABEL` ||
|| **EDIT_FORM_LABEL**
```string | lang_map``` | Label in the edit form. Strings and `lang_map` are processed in the same way as for `LIST_FILTER_LABEL` ||
|| **ERROR_MESSAGE**
```string | lang_map``` | Error message. Strings and `lang_map` are processed in the same way as for `LIST_FILTER_LABEL` ||
|| **HELP_MESSAGE**
```string | lang_map``` | Help message. Strings and `lang_map` are processed in the same way as for `LIST_FILTER_LABEL` ||
|#

### Parameter SETTINGS {#settings}

The method changes only the settings described below. It does not change settings for `date` fields or types without a tab below and does not return an error for them.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value ||
    || **ROWS**
    [`integer`](../../../data-types.md) | Number of rows in the input field. The value is limited to the range from `1` to `50`, inclusive
    ||
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
    [`integer`](../../../data-types.md) | Number precision. A negative value is stored as `2`. A value greater than `12` is stored as `12` ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../../data-types.md) | Default value, where `1` is yes, `0` is no.

    The passed value is converted to `1` or `0`:
    - `>= 1` -> 1
    - `<= 0` -> 0
    ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance. Possible values:
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
    [`object`](../../../data-types.md)  | Default value. Object format:
    ```
    {
        VALUE: datetime,
        TYPE: 'NONE'|'NOW'|'FIXED',
    }
    ```

    where:
    - `VALUE` — default value of type `datetime`
    - `TYPE` — default value type:
        - `NONE` — do not set a default value
        - `NOW` — use current time/date
        - `FIXED` — use time/date from `VALUE`

    If `VALUE` is omitted, an empty string is used. If `TYPE` is omitted or empty, `NONE` is used
    ||
    |#

- enumeration

    #|
    || **Name**
    `type` | **Description** ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance. Possible values:
    - `LIST` — list
    - `UI` — autocomplete list
    - `CHECKBOX` — checkboxes
    - `DIALOG` — entity selection dialog
    ||
    || **LIST_HEIGHT**
    [`integer`](../../../data-types.md) | List height. Must be greater than 0 ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **IBLOCK_ID**
    [`integer`](../../../data-types.md) | Infoblock identifier. If a value less than or equal to `0` is passed, the method returns the `ERROR_CORE` error ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance. Possible values:
    - `DIALOG` — dialog
    - `UI` — autocomplete list
    - `LIST` — list
    - `CHECKBOX` — checkboxes
    ||
    || **LIST_HEIGHT**
    [`integer`](../../../data-types.md) | List height. Must be greater than 0
    ||
    || **ACTIVE_FILTER**
    [`char`](../../../data-types.md) | Whether to show items with the activity flag enabled. Possible values:
    - `Y` — yes
    - `N` — no
    ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
    || **ENTITY_TYPE**
    [`string`](../../../data-types.md) | Dictionary type identifier.

    Use [`crm.status.entity.types`](../../status/crm-status-entity-types.md) to find possible values ||
    |#

- crm

    The provided settings are merged with the saved settings. If no links remain enabled after the update, `LEAD = Y` is set automatically.

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`char`](../../../data-types.md) | Whether binding to [Leads](../../leads/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no
    ||
    || **CONTACT**
    [`char`](../../../data-types.md) | Whether binding to [Contacts](../index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no
    ||
    || **COMPANY**
    [`char`](../../../data-types.md) | Whether binding to [Companies](../../companies/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no
    ||
    || **DEAL**
    [`char`](../../../data-types.md) | Whether binding to [Deals](../../deals/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no
    ||
    |#

{% endlist %}

### uf_enum_element Type {#uf_enum_element}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../../data-types.md) | Identifier of the list item. To change an existing item, pass its `ID` and a non-empty `VALUE`. If `ID` is passed without `VALUE` or with an empty `VALUE`, the item is deleted. Without `ID`, a new item is added.

The identifier can be obtained using the method [`crm.contact.userfield.get`](./crm-contact-userfield-get.md#uf_enum_element)
||
|| **DEL**
[`char`](../../../data-types.md) | Flag necessary for deleting a list item. Makes sense only when passing `ID`.

Possible values:
- `Y` — delete
- `N` — do not delete

Default is `N`
||
|| **VALUE**
[`string`](../../../data-types.md) | Value of the list element ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. If omitted or less than one, a new item receives `500` ||
|| **DEF**
[`char`](../../../data-types.md) | Is the list item the default value? Possible values:
- `Y` — yes
- `N` — no

For a multiple field, multiple `DEF = Y` is allowed. For a non-multiple field, the first passed list item with `DEF = Y` is the default value ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code of the value, unique within the field's list items. To retain the external code of an item being changed, pass it explicitly. Otherwise, a new code is generated for a non-empty `VALUE`. [crm.contact.userfield.list](./crm-contact-userfield-list.md) returns the current item `XML_ID`. `crm.contact.userfield.get` does not return it ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

### Example of Updating a String Type Custom Field

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"N","SETTINGS":{"DEFAULT_VALUE":"Hello, world! Default value (changed)","ROWS":10},"SORT":2000,"EDIT_IN_LIST":"N","LIST_FILTER_LABEL":"Hello, world! Filter (changed)","LIST_COLUMN_LABEL":{"en":"Hello, World! Column (changed)","ru":"Hello, world! Column (changed)","de":"Hallo, Welt! Spalte (geändert)"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit (changed)","ru":"Hello, world! Edit (changed)","de":"Hallo, Welt! Bearbeiten (geändert)"},"ERROR_MESSAGE":{"en":"Hello, World! Error (changed)","ru":"Hello, world! Error (changed)","de":"Hallo, Welt! Fehler (geändert)"},"HELP_MESSAGE":{"en":"Hello, World! Help (changed)","ru":"Hello, world! Help (changed)","de":"Hallo, Welt! Hilfe (geändert)"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"N","SETTINGS":{"DEFAULT_VALUE":"Hello, world! Default value (changed)","ROWS":10},"SORT":2000,"EDIT_IN_LIST":"N","LIST_FILTER_LABEL":"Hello, world! Filter (changed)","LIST_COLUMN_LABEL":{"en":"Hello, World! Column (changed)","ru":"Hello, world! Column (changed)","de":"Hallo, Welt! Spalte (geändert)"},"EDIT_FORM_LABEL":{"en":"Hello, World! Edit (changed)","ru":"Hello, world! Edit (changed)","de":"Hallo, Welt! Bearbeiten (geändert)"},"ERROR_MESSAGE":{"en":"Hello, World! Error (changed)","ru":"Hello, world! Error (changed)","de":"Hallo, Welt! Fehler (geändert)"},"HELP_MESSAGE":{"en":"Hello, World! Help (changed)","ru":"Hello, world! Help (changed)","de":"Hallo, Welt! Hilfe (geändert)"}}, "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.userfield.update
    ```

- BX24.js

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
                    "ru": "Hello, world! Column (changed)",
                    "de": "Hallo, Welt! Spalte (geändert)"
                },
                EDIT_FORM_LABEL: {
                    "en": "Hello, World! Edit (changed)",
                    "ru": "Hello, world! Edit (changed)",
                    "de": "Hallo, Welt! Bearbeiten (geändert)"
                },
                ERROR_MESSAGE: {
                    "en": "Hello, World! Error (changed)",
                    "ru": "Hello, world! Error (changed)",
                    "de": "Hallo, Welt! Fehler (geändert)"
                },
                HELP_MESSAGE: {
                    "en": "Hello, World! Help (changed)",
                    "ru": "Hello, world! Help (changed)",
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
    try {
        $contactUserfieldItemId = 536; // Example ID
        $userfieldFieldsToUpdate = [
            'SORT' => '100',
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
            'SETTINGS' => ['DEFAULT_VALUE' => 'Hello, world!', 'ROWS' => 10],
        ];

        $result = $serviceBuilder
            ->getCRMScope()
            ->contactUserfield()
            ->update($contactUserfieldItemId, $userfieldFieldsToUpdate);

        if ($result->isSuccess()) {
            var_export($result->getCoreResponse()->getResponseData()->getResult());
        } else {
            print("Update failed.");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

- PHP CRest

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
                    'ru' => "Hello, world! Column (changed)",
                    'de' => "Hallo, Welt! Spalte (geändert)"
                ],
                'EDIT_FORM_LABEL' => [
                    'en' => "Hello, World! Edit (changed)",
                    'ru' => "Hello, world! Edit (changed)",
                    'de' => "Hallo, Welt! Bearbeiten (geändert)"
                ],
                'ERROR_MESSAGE' => [
                    'en' => "Hello, World! Error (changed)",
                    'ru' => "Hello, world! Error (changed)",
                    'de' => "Hallo, Welt! Fehler (geändert)"
                ],
                'HELP_MESSAGE' => [
                    'en' => "Hello, World! Help (changed)",
                    'ru' => "Hello, world! Help (changed)",
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

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

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
                    "ru": "Hello, world! Column (changed)",
                    "de": "Hallo, Welt! Spalte (geändert)",
                },
                "EDIT_FORM_LABEL": {
                    "en": "Hello, World! Edit (changed)",
                    "ru": "Hello, world! Edit (changed)",
                    "de": "Hallo, Welt! Bearbeiten (geändert)",
                },
                "ERROR_MESSAGE": {
                    "en": "Hello, World! Error (changed)",
                    "ru": "Hello, world! Error (changed)",
                    "de": "Hallo, Welt! Fehler (geändert)",
                },
                "HELP_MESSAGE": {
                    "en": "Hello, World! Help (changed)",
                    "ru": "Hello, world! Help (changed)",
                    "de": "Hallo, Welt! Hilfe (geändert)",
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.contact.userfield.update", b24.Params{
    	"id": 536,
    	"fields": b24.Params{
    		"MANDATORY":   "N",
    		"SHOW_FILTER": "N",
    		"SETTINGS": b24.Params{
    			"DEFAULT_VALUE": "Hello, world! Default value (changed)",
    			"ROWS":          10,
    		},
    		"SORT":              2000,
    		"EDIT_IN_LIST":      "N",
    		"LIST_FILTER_LABEL": "Hello, world! Filter (changed)",
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
    	return fmt.Errorf("crm.contact.userfield.update: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

### Example of Modifying a List Type Custom Field

Current list items from the `crm.contact.userfield.list` response:

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

Modify the list as follows. Replace `536` with the identifier of a list-type custom field.

- remove list items with `ID = 115` and `ID = 116`
- modify the list item with `ID  = 117`:
    - `VALUE`: "List item #3" -> "List item #3 (modified)"
    - `SORT`: 300 -> 50
- add a new list item "List item #5"

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"Y","LIST":[{"ID":115,"DEL":"Y"},{"ID":116,"DEL":"Y"},{"ID":117,"XML_ID":"XML_ID_3","VALUE":"List item #3 (changed)","SORT":50},{"VALUE":"List item #5","XML_ID":"XML_ID_5","SORT":500}],"SETTINGS":{"DISPLAY":"DIALOG","LIST_HEIGHT":3},"SORT":1000}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":536,"fields":{"MANDATORY":"N","SHOW_FILTER":"Y","LIST":[{"ID":115,"DEL":"Y"},{"ID":116,"DEL":"Y"},{"ID":117,"XML_ID":"XML_ID_3","VALUE":"List item #3 (changed)","SORT":50},{"VALUE":"List item #5","XML_ID":"XML_ID_5","SORT":500}],"SETTINGS":{"DISPLAY":"DIALOG","LIST_HEIGHT":3},"SORT":1000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.userfield.update
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.contact.userfield.update',
        {
            id: 536,
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
                        XML_ID: "XML_ID_3",
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.userfield.update',
        [
            'id' => 536,
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
                        'XML_ID' => "XML_ID_3",
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

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

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
                "LIST": [
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
                        "XML_ID": "XML_ID_3",
                        "VALUE": "List item #3 (changed)",
                        "SORT": 50,
                    },
                    {
                        "VALUE": "List item #5",
                        "XML_ID": "XML_ID_5",
                        "SORT": 500,
                    },
                ],
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.contact.userfield.update", b24.Params{
        "id": 536,
    	"fields": b24.Params{
    		"MANDATORY":   "N",
    		"SHOW_FILTER": "Y",
    		"LIST": []b24.Params{
    			{
    				"ID":  115,
    				"DEL": "Y",
    			},
    			{
    				"ID":  116,
    				"DEL": "Y",
    			},
			{
				"ID":    117,
				"XML_ID": "XML_ID_3",
				"VALUE": "List item #3 (changed)",
				"SORT":  50,
			},
    			{
    				"VALUE":  "List item #5",
    				"XML_ID": "XML_ID_5",
    				"SORT":   500,
    			},
    		},
    		"SETTINGS": b24.Params{
    			"DISPLAY":     "DIALOG",
    			"LIST_HEIGHT": 3,
    		},
    		"SORT": 1000,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.contact.userfield.update: %w", err)
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

Field validation error messages end with the HTML tag `<br>`. The table omits it.

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Parameter 'fields' must be array.` | The provided `fields` is not an object ||
|| `400` | Empty value | `ID is not defined or invalid.` | The passed `id` is less than or equal to zero or was not provided ||
|| `400` | Empty value | `Access denied.` | The user does not have the "Edit CRM preferences" permission in CRM, or the field with the provided `id` does not belong to contacts ||
|| `400` | `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found.` | The user field with the passed `id` does not exist ||
|| `400` | `ERROR_CORE` | A list item with XML_ID=`xml_id` already exists. | The provided `XML_ID` values for list items are not unique ||
|| `400` | `ERROR_CORE` | Select an information block to link to the field | For `iblock_section` or `iblock_element`, `SETTINGS.IBLOCK_ID` is no greater than `0` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-userfield-add.md)
- [{#T}](./crm-contact-userfield-get.md)
- [{#T}](./crm-contact-userfield-list.md)
- [{#T}](./crm-contact-userfield-delete.md)

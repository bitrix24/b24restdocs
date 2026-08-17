# Edit an Existing Contact Custom Field crm.contact.userfield.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

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

An incorrect field in `fields` will be ignored.

Only those fields that need to be changed should be passed in `fields` ||
|#

### Parameter fields {#parameter-fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **MANDATORY**
[`boolean`](../../../data-types.md) | Is the field mandatory? Possible values:
- `Y` — yes
- `N` — no
||
|| **SHOW_FILTER**
[`boolean`](../../../data-types.md) | Should the field be shown in the filter? Possible values:
- `Y` — yes
- `N` — no
||
|| **XML_ID**
[`string`](../../../data-types.md) | External code ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional field parameters. Each field type (`USER_TYPE_ID`) has its own set of available settings, which are described [below](#settings).

The field only overwrites the passed values ||
|| **LIST**
[`uf_enum_element[]`](#uf_enum_element) | List of possible values for the custom field of type `enumeration`. For custom fields of other types, this parameter is meaningless ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. Must be greater than zero ||
|| **SHOW_IN_LIST**
[`boolean`](../../../data-types.md) | Should the user field be shown in the list?

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no
||
|| **EDIT_IN_LIST**
[`boolean`](../../../data-types.md) | Allow user editing? Possible values:
- `Y` — yes
- `N` — no
||
|| **IS_SEARCHABLE**
[`boolean`](../../../data-types.md) | Are the field values searchable?

This parameter does not affect anything within `crm`.

Possible values:
- `Y` — yes
- `N` — no
||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md) | Filter label in the list.

When a string is passed, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md) | Header in the list.

When a string is passed, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md) | Label in the edit form.

When a string is passed, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md) | Error message.

When passing a string, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md)\|[`lang_map`](../../data-types.md) | Help message.

When passing a string, it is set for each language.

For languages where no value is explicitly specified, `''` will be recorded.

The field completely overwrites the previous value ||
|#

### Parameter SETTINGS {#settings}

Each custom field type has its own set of additional configurations. This method supports changing only those described below.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value ||
    || **ROWS**
    [`integer`](../../../data-types.md) | Number of rows in the input field. Must be greater than 0 and less than 50.

    If a value <= 0 is passed, the value `1` will be set.

    If a value >= 50 is passed, the value `50` will be set
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
    [`integer`](../../../data-types.md) | Number precision. Must be greater than or equal to 0.

    If an invalid value is passed, the value `2` will be set ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../../data-types.md) | Default value, where `1` is yes, `0` is no.

    When a value is passed, the value will be set according to the rule:
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
    [`string`](../../../data-types.md) | Appearance. Possible values:
    - `LIST` — list
    - `UI` — autocomplete list
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
    [`string`](../../../data-types.md) | Infoblock type identifier ||
    || **IBLOCK_ID**
    [`string`](../../../data-types.md) | Infoblock identifier ||
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
    [`boolean`](../../../data-types.md) | Whether to show items with the activity flag enabled. Possible values:
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

    If none of the following options are passed, linking to leads (`LEAD = Y`) will be enabled by default

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`boolean`](../../../data-types.md) | Whether binding to [Leads](../../leads/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no
    ||
    || **CONTACT**
    [`boolean`](../../../data-types.md) | Whether binding to [Contacts](../index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no
    ||
    || **COMPANY**
    [`boolean`](../../../data-types.md) | Whether binding to [Companies](../../companies/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no
    ||
    || **DEAL**
    [`boolean`](../../../data-types.md) | Whether binding to [Deals](../../deals/index.md) is enabled. Possible values:
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
[`string`](../../../data-types.md) | Identifier of the list item. When passing this parameter, the corresponding list item will be changed; otherwise, a new list item will be added.

The identifier can be obtained using the method [`crm.contact.userfield.get`](./crm-contact-userfield-get.md#uf_enum_element)
||
|| **DEL**
[`boolean`](../../../data-types.md) | Flag necessary for deleting a list item. Makes sense only when passing `ID`. 

Possible values:
`Y` — delete
`N` — do not delete

Default is `N`
||
|| **VALUE**
[`string`](../../../data-types.md) | Value of the list element ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index. Must be greater than or equal to 0 ||
|| **DEF**
[`boolean`](../../../data-types.md) | Is the list item the default value? Possible values:
- `Y` — yes
- `N` — no

For a multiple field, multiple `DEF = Y` is allowed. For a non-multiple field, the first passed list item with `DEF = Y` will be considered the default value ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code of the value. Must be unique within the elements of the user field list ||
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

Modify it as follows:
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

- BX24.js

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

- PHP CRest

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

#|
|| **Code** | **Description** | **Value** ||
|| Empty value | `Parameter 'fields' must be array` | The provided `fields` is not an object ||
|| Empty value | `ID is not defined or invalid`     | The passed `id` is less than zero or not passed at all ||
|| Empty value | `Access denied`                    | Occurs when:
- the user does not have administrative rights
- the user attempts to delete a custom field not associated with contacts ||
|| `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found` | The user field with the passed `id` does not exist ||
|| `ERROR_CORE`               | A list item with XML_ID=`XML_ID` already exists | The passed `XML_ID` for the list element must be unique within the elements of the user field list ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-userfield-add.md)
- [{#T}](./crm-contact-userfield-get.md)
- [{#T}](./crm-contact-userfield-list.md)
- [{#T}](./crm-contact-userfield-delete.md)

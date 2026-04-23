# Add Custom Field userfieldconfig.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`userfieldconfig`](../../../scopes/permissions.md), module scope from `moduleId` (for example, [`crm`](../../../scopes/permissions.md))
>
> Who can execute the method: a user with permission to modify object settings in the `moduleId` module (for `crm` — permission "Allow to modify settings")

The `userfieldconfig.add` method adds a new custom field.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../../../data-types.md) | Module identifier where the field is created ||
|| **field***
[`object`](../../../data-types.md) | Object with custom field settings [(detailed description)](#field) ||
|#

### Parameter field {#field}

#|
|| **Name**
`type` | **Description** ||
|| **entityId***
[`string`](../../../data-types.md) | Identifier of the object for which the field is created. The format depends on the module, for example, `CRM_7` for SPA ||
|| **fieldName***
[`string`](../../../data-types.md) | Field code in the format `UF_{OBJECT_IDENTIFIER}_{POSTFIX}`. The code must be unique within the object. Allowed characters are `A-Z`, `0-9`, `_`. Maximum length of the code is 50 characters ||
|| **userTypeId***
[`string`](../../../data-types.md) | Identifier of the field type. The list of available types is returned by the [userfieldconfig.getTypes](./userfieldconfig-get-types.md) method ||
|| **xmlId**
[`string`](../../../data-types.md) | External identifier of the field ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index. Default is `100` ||
|| **multiple**
[`boolean`](../../../data-types.md) | Whether the field is multiple. Possible values: `Y` or `N`. Default is `N` ||
|| **mandatory**
[`boolean`](../../../data-types.md) | Whether the field is mandatory. Possible values: `Y` or `N`. Default is `N` ||
|| **showFilter**
[`boolean`](../../../data-types.md) | Whether to show the field in the filter. Possible values: `Y` or `N`. Default is `N` ||
|| **editInList**
[`boolean`](../../../data-types.md) | Whether to allow editing the value in the list. Possible values: `Y` or `N` ||
|| **isSearchable**
[`boolean`](../../../data-types.md) | Whether the field values participate in the search. Possible values: `Y` or `N` ||
|| **settings**
[`object`](../../../data-types.md) | Additional field settings. The set of keys depends on `userTypeId` [(detailed description)](#settings) ||
|| **editFormLabel**
[`string`](../../../data-types.md)\|[`lang_map`](../../../data-types.md#lang_map) | Label in the edit form. When a string is passed, it is used as a general value; when a `lang_map` is passed, labels can be set by languages ||
|| **helpMessage**
[`string`](../../../data-types.md)\|[`lang_map`](../../../data-types.md#lang_map) | Help text. When a string is passed, it is used as a general value; when a `lang_map` is passed, help texts can be set by languages ||
|| **enum**
[`uf_enum_element[]`](#uf_enum_element) | Value options for fields of type `enumeration` ||
|#

The method uses a fixed set of keys in `field` (see the table above).

Incorrect and unsupported keys in `field` are ignored.

Keys `showInList`, `listColumnLabel`, `listFilterLabel`, `errorMessage`, `label` are not processed by the `userfieldconfig.add` method, even if passed in `field`.

### Parameter settings {#settings}

Each field type has its own set of keys in `settings`.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value ||
    || **ROWS**
    [`integer`](../../../data-types.md) | Number of rows in the input field, must be greater than 0 ||
    || **SIZE**
    [`integer`](../../../data-types.md) | Width of the input field ||
    || **REGEXP**
    [`string`](../../../data-types.md) | Regular expression for validation ||
    || **MIN_LENGTH**
    [`integer`](../../../data-types.md) | Minimum string length ||
    || **MAX_LENGTH**
    [`integer`](../../../data-types.md) | Maximum string length ||
    |#

- integer

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../../data-types.md) | Default value ||
    || **SIZE**
    [`integer`](../../../data-types.md) | Width of the input field ||
    || **MIN_VALUE**
    [`integer`](../../../data-types.md) | Minimum value ||
    || **MAX_VALUE**
    [`integer`](../../../data-types.md) | Maximum value ||
    |#

- double

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`double`](../../../data-types.md) | Default value ||
    || **PRECISION**
    [`integer`](../../../data-types.md) | Number precision, must be greater than or equal to 0 ||
    || **SIZE**
    [`integer`](../../../data-types.md) | Width of the input field ||
    || **MIN_VALUE**
    [`double`](../../../data-types.md) | Minimum value ||
    || **MAX_VALUE**
    [`double`](../../../data-types.md) | Maximum value ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../../data-types.md) | Default value, where `1` = yes and `0` = no ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance, possible values: `CHECKBOX`, `RADIO`, `DROPDOWN` ||
    || **LABEL**
    [`string`](../../../data-types.md) | Label for the Yes value ||
    || **LABEL_CHECKBOX**
    [`string`](../../../data-types.md) | Label for `CHECKBOX` mode ||
    |#

- date|datetime

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`object`](../../../data-types.md) | Default value in the format `{VALUE, TYPE}`, where `TYPE`: `NONE`, `NOW`, `FIXED` ||
    || **USE_SECOND**
    [`boolean`](../../../data-types.md) | Use seconds in the `datetime` field ||
    || **USE_TIMEZONE**
    [`boolean`](../../../data-types.md) | Use timezone in the `datetime` field ||
    |#

- money

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value in the format `{VALUE}|{CURRENCY}` ||
    |#

- url

    #|
    || **Name**
    `type` | **Description** ||
    || **POPUP**
    [`boolean`](../../../data-types.md) | Open link in a new window ||
    || **SIZE**
    [`integer`](../../../data-types.md) | Width of the input field ||
    || **MIN_LENGTH**
    [`integer`](../../../data-types.md) | Minimum length of the value ||
    || **MAX_LENGTH**
    [`integer`](../../../data-types.md) | Maximum length of the value ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value ||
    || **ROWS**
    [`integer`](../../../data-types.md) | Number of rows in the input field ||
    |#

- address

    #|
    || **Name**
    `type` | **Description** ||
    || **SHOW_MAP**
    [`boolean`](../../../data-types.md) | Show map for the address ||
    |#

- file

    #|
    || **Name**
    `type` | **Description** ||
    || **SIZE**
    [`integer`](../../../data-types.md) | Width of the input field ||
    || **LIST_WIDTH**
    [`integer`](../../../data-types.md) | Width of the preview in the list ||
    || **LIST_HEIGHT**
    [`integer`](../../../data-types.md) | Height of the preview in the list ||
    || **MAX_SHOW_SIZE**
    [`integer`](../../../data-types.md) | Maximum file size for display ||
    || **MAX_ALLOWED_SIZE**
    [`integer`](../../../data-types.md) | Maximum allowable file size ||
    || **EXTENSIONS**
    [`string[]`](../../../data-types.md) | List of allowed extensions ||
    || **TARGET_BLANK**
    [`boolean`](../../../data-types.md) | Open file in a new tab ||
    |#

- enumeration

    #|
    || **Name**
    `type` | **Description** ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance, possible values: `LIST`, `UI`, `CHECKBOX`, `DIALOG` ||
    || **LIST_HEIGHT**
    [`integer`](../../../data-types.md) | Height of the list, must be greater than 0 ||
    || **CAPTION_NO_VALUE**
    [`string`](../../../data-types.md) | Label for empty value ||
    || **SHOW_NO_VALUE**
    [`boolean`](../../../data-types.md) | Show empty value ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **DISPLAY**
    [`string`](../../../data-types.md) | Appearance, possible values: `DIALOG`, `UI`, `LIST`, `CHECKBOX` ||
    || **LIST_HEIGHT**
    [`integer`](../../../data-types.md) | Height of the list, must be greater than 0 ||
    || **IBLOCK_ID**
    [`integer`](../../../data-types.md) | Identifier of the information block ||
    || **DEFAULT_VALUE**
    [`string`](../../../data-types.md) | Default value ||
    || **ACTIVE_FILTER**
    [`boolean`](../../../data-types.md) | Use only active elements ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | Identifier of the CRM reference type. Possible values can be obtained using the [`crm.status.entity.types`](../../status/crm-status-entity-types.md) method ||
    |#

- crm

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`boolean`](../../../data-types.md) | Enable binding to leads ||
    || **CONTACT**
    [`boolean`](../../../data-types.md) | Enable binding to contacts ||
    || **COMPANY**
    [`boolean`](../../../data-types.md) | Enable binding to companies ||
    || **DEAL**
    [`boolean`](../../../data-types.md) | Enable binding to deals ||
    || **QUOTE**
    [`boolean`](../../../data-types.md) | Enable binding to estimates ||
    || **ORDER**
    [`boolean`](../../../data-types.md) | Enable binding to orders ||
    || **SMART_INVOICE**
    [`boolean`](../../../data-types.md) | Enable binding to invoices ||
    || **DYNAMIC_***
    [`boolean`](../../../data-types.md) | Enable binding to SPA with a specific `typeId` ||
    |#

- employee

    Separate settings in `settings` for the `employee` type are not used.

- rest_*

    Settings are defined by the custom field type handler.

{% endlist %}

### Type uf_enum_element {#uf_enum_element}

#|
|| **Name**
`type` | **Description** ||
|| **value***
[`string`](../../../data-types.md) | Value of the list option ||
|| **def**
[`boolean`](../../../data-types.md) | Default value flag (`Y`/`N`) ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index of the option ||
|| **xmlId**
[`string`](../../../data-types.md) | External identifier of the option ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "moduleId": "crm",
        "field": {
          "entityId": "CRM_7",
          "fieldName": "UF_CRM_7_NEW_REST_LIST_2026",
          "userTypeId": "enumeration",
          "multiple": "Y",
          "editFormLabel": {
            "en": "List of characteristics"
          },
          "enum": [
            { "value": "Characteristic 1", "def": "N", "sort": 100 },
            { "value": "Characteristic 2", "def": "Y", "sort": 200 }
          ]
        }
      }' \
      "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldconfig.add"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "moduleId": "crm",
        "field": {
          "entityId": "CRM_7",
          "fieldName": "UF_CRM_7_NEW_REST_LIST_2026",
          "userTypeId": "enumeration",
          "multiple": "Y",
          "editFormLabel": {
            "en": "List of characteristics"
          },
          "enum": [
            { "value": "Characteristic 1", "def": "N", "sort": 100 },
            { "value": "Characteristic 2", "def": "Y", "sort": 200 }
          ]
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put_your_bitrix24_address**/rest/userfieldconfig.add"
    ```

- JS

    ```js
    const endpoint = "https://**put_your_bitrix24_address**/rest/userfieldconfig.add.json";

    fetch(endpoint, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
            auth: "**put_access_token_here**",
            moduleId: "crm",
            field: {
                entityId: "CRM_7",
                fieldName: "UF_CRM_7_NEW_REST_LIST_2026",
                userTypeId: "enumeration",
                multiple: "Y",
                editFormLabel: {
                    en: "List of characteristics",
                },
                enum: [
                    { value: "Characteristic 1", def: "N", sort: 100 },
                    { value: "Characteristic 2", def: "Y", sort: 200 },
                ],
            },
        }),
    })
        .then((response) => response.json())
        .then((data) => console.log(data))
        .catch((error) => console.error(error));
    ```

- PHP

    ```php
    $payload = [
        'auth' => '**put_access_token_here**',
        'moduleId' => 'crm',
        'field' => [
            'entityId' => 'CRM_7',
            'fieldName' => 'UF_CRM_7_NEW_REST_LIST_2026',
            'userTypeId' => 'enumeration',
            'multiple' => 'Y',
            'editFormLabel' => [
                'en' => 'List of characteristics',
            ],
            'enum' => [
                ['value' => 'Characteristic 1', 'def' => 'N', 'sort' => 100],
                ['value' => 'Characteristic 2', 'def' => 'Y', 'sort' => 200],
            ],
        ],
    ];

    $curl = curl_init('https://**put_your_bitrix24_address**/rest/userfieldconfig.add.json');
    curl_setopt_array($curl, [
        CURLOPT_POST => true,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_HTTPHEADER => ['Content-Type: application/json'],
        CURLOPT_POSTFIELDS => json_encode($payload),
    ]);

    $result = curl_exec($curl);
    curl_close($curl);

    print_r($result);
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "userfieldconfig.add",
        {
            moduleId: "crm",
            field: {
                entityId: "CRM_7",
                fieldName: "UF_CRM_7_NEW_REST_LIST_2026",
                userTypeId: "enumeration",
                multiple: "Y",
                editFormLabel: {
                    en: "List of characteristics",
                },
                enum: [
                    { value: "Characteristic 1", def: "N", sort: 100 },
                    { value: "Characteristic 2", def: "Y", sort: 200 },
                ],
            },
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldconfig.add',
        [
            'moduleId' => 'crm',
            'field' => [
                'entityId' => 'CRM_7',
                'fieldName' => 'UF_CRM_7_NEW_REST_LIST_2026',
                'userTypeId' => 'enumeration',
                'multiple' => 'Y',
                'editFormLabel' => [
                    'en' => 'List of characteristics',
                ],
                'enum' => [
                    ['value' => 'Characteristic 1', 'def' => 'N', 'sort' => 100],
                    ['value' => 'Characteristic 2', 'def' => 'Y', 'sort' => 200],
                ],
            ],
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "field": {
            "id": "6953",
            "entityId": "CRM_7",
            "fieldName": "UF_CRM_7_NEW_REST_LIST_2026",
            "userTypeId": "enumeration",
            "xmlId": null,
            "sort": "100",
            "multiple": "Y",
            "mandatory": "N",
            "showFilter": "N",
            "showInList": "Y",
            "editInList": "Y",
            "isSearchable": "N",
            "settings": {
                "DISPLAY": "LIST",
                "LIST_HEIGHT": 1,
                "CAPTION_NO_VALUE": "",
                "SHOW_NO_VALUE": "Y"
            },
            "languageId": {
                "en": "en",
                "ru": "ru"
            },
            "editFormLabel": {
                "en": "List of characteristics"
            },
            "listColumnLabel": {
                "en": null,
                "ru": null
            },
            "listFilterLabel": {
                "en": null,
                "ru": null
            },
            "errorMessage": {
                "en": null,
                "ru": null
            },
            "helpMessage": {
                "en": null,
                "ru": null
            },
            "enum": [
                {
                    "id": "3363",
                    "userFieldId": "6953",
                    "value": "Characteristic 1",
                    "def": "N",
                    "sort": "100",
                    "xmlId": "56dff18efcfe25f3bae0117a6b372567"
                },
                {
                    "id": "3365",
                    "userFieldId": "6953",
                    "value": "Characteristic 2",
                    "def": "Y",
                    "sort": "200",
                    "xmlId": "42e3ebcf5506a65283bf3bf510d8f05a"
                }
            ]
        }
    },
    "time": {
        "start": 1724239307.903115,
        "finish": 1724239308.567422,
        "duration": 0.6643068790435791,
        "processing": 0.20090818405151367,
        "date_start": "2024-08-21T13:21:47+02:00",
        "date_finish": "2024-08-21T13:21:48+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **field**
[`object`](../../../data-types.md) | Settings of the created custom field [(detailed description)](#result_field) ||
|#

##### Object field {#result_field}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the field settings ||
|| **entityId**
[`string`](../../../data-types.md) | Identifier of the object ||
|| **fieldName**
[`string`](../../../data-types.md) | Field code ||
|| **userTypeId**
[`string`](../../../data-types.md) | Identifier of the field type ||
|| **xmlId**
[`string`](../../../data-types.md) | External identifier of the field ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **multiple**
[`boolean`](../../../data-types.md) | Flag for multiple values (`Y`/`N`) ||
|| **mandatory**
[`boolean`](../../../data-types.md) | Flag for mandatory field (`Y`/`N`) ||
|| **showFilter**
[`boolean`](../../../data-types.md) | Flag for showing the field in the filter ||
|| **showInList**
[`boolean`](../../../data-types.md) | Flag for showing the field in the list ||
|| **editInList**
[`boolean`](../../../data-types.md) | Flag for editing in the list ||
|| **isSearchable**
[`boolean`](../../../data-types.md) | Flag for participation in search ||
|| **settings**
[`object`](../../../data-types.md) | Additional field settings [(detailed description)](#settings). The set of keys depends on `userTypeId` ||
|| **languageId**
[`object`](../../../data-types.md) | Languages for which field labels are set ||
|| **editFormLabel**
[`lang_map`](../../../data-types.md) | Labels in the edit form ||
|| **listColumnLabel**
[`lang_map`](../../../data-types.md) | Column labels in the list ||
|| **listFilterLabel**
[`lang_map`](../../../data-types.md) | Filter labels ||
|| **errorMessage**
[`lang_map`](../../../data-types.md) | Error message text ||
|| **helpMessage**
[`lang_map`](../../../data-types.md) | Help text for the field ||
|| **enum**
[`object[]`](../../../data-types.md) | Value options. This field is returned only for `userTypeId = enumeration` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The 'FIELD_NAME' field is not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | Access denied | Insufficient permissions to create a custom field ||
|| `-` | You cannot create custom fields | This error may occur if `field.fieldName` does not start with `UF_{entityId}_` ||
|| `-` | The 'USER_TYPE_ID' field is not found | Mandatory `field.userTypeId` not provided ||
|| `-` | The 'FIELD_NAME' field is not found | Mandatory `field.fieldName` not provided ||
|| `-` | Field ... already exists | The provided `field.fieldName` is already in use for this object ||
|| `-` | Fail to create new user field | Error creating the field on the server side ||
|| `-` | Fail to save enumeration field values | Error saving list values for type `enumeration` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldconfig-update.md)
- [{#T}](./userfieldconfig-get.md)
- [{#T}](./userfieldconfig-list.md)
- [{#T}](./userfieldconfig-delete.md)
- [{#T}](./userfieldconfig-get-types.md)
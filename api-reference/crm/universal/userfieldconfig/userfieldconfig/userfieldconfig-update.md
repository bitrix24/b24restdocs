# Update User Field `userfieldconfig.update`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`userfieldconfig`](../../../../scopes/permissions.md), module scope from `moduleId` (for example, [`crm`](../../../../scopes/permissions.md))
>
> Who can execute the method: a user with permission to modify object settings in the `moduleId` module (for `crm` — permission "Allow to modify settings")

The method `userfieldconfig.update` updates the settings of an existing user field.

## Method Parameters

{% include [Parameter Note](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../../../data-types.md) | Identifier of the module where the field is located ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the user field settings ||
|| **field***
[`object`](../../../data-types.md) | Object with new field settings [(detailed description)](#field) ||
|#

### Field Parameter {#field}

#|
|| **Name**
`type` | **Description** ||
|| **xmlId**
[`string`](../../../data-types.md) | External identifier of the field ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **mandatory**
[`boolean`](../../../data-types.md) | Mandatory field flag (`Y`/`N`) ||
|| **showFilter**
[`boolean`](../../../data-types.md) | Show field in filter flag (`Y`/`N`) ||
|| **isSearchable**
[`boolean`](../../../data-types.md) | Field participation in search flag (`Y`/`N`) ||
|| **editFormLabel**
[`lang_map`](../../../data-types.md) | Labels in the edit form by languages ||
|| **helpMessage**
[`lang_map`](../../../data-types.md) | Help text by languages ||
|| **settings**
[`object`](../../../data-types.md) | Additional field settings. The set of keys depends on the field type [(detailed description)](#settings) ||
|| **enum**
[`uf_enum_element[]`](#uf_enum_element) | List of value options for the `enumeration` field type.

To apply changes to `enum`, `field.userTypeId` must be set to `enumeration` ||
|| **userTypeId**
[`string`](../../../data-types.md) | Field type.

Used when updating `enum`, in this case, pass `enumeration` ||
|#

### Settings Parameter {#settings}

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
    [`integer`](../../../data-types.md) | Maximum allowed file size ||
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
    [`string`](../../../data-types.md) | Identifier of the CRM reference type. Possible values can be obtained using the method [`crm.status.entity.types`](../../../status/crm-status-entity-types.md) ||
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

    Settings are defined by the user-defined field handler.

{% endlist %}

### Type `uf_enum_element` {#uf_enum_element}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the existing option. Used for updating or deleting ||
|| **value**
[`string`](../../../data-types.md) | Value of the option ||
|| **def**
[`boolean`](../../../data-types.md) | Default value flag (`Y`/`N`) ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **xmlId**
[`string`](../../../data-types.md) | External identifier of the option ||
|| **del**
[`boolean`](../../../data-types.md) | Flag for deleting the existing option (`Y`/`N`) ||
|#

{% note info "" %}

Some field parameters cannot be changed after creation. If they are passed in `field`, the change will not be applied.

{% endnote %}

## Code Examples

{% include [Example Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","id":7095,"field":{"mandatory":"Y","editFormLabel":{"en":"List of characteristics (updated)"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldconfig.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","id":7095,"field":{"mandatory":"Y","editFormLabel":{"en":"List of characteristics (updated)"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/userfieldconfig.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'userfieldconfig.update',
    		{
    			moduleId: 'crm',
    			id: 7095,
    			field: {
    				mandatory: 'Y',
    				editFormLabel: {
    					en: 'List of characteristics (updated)',
    				},
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'userfieldconfig.update',
                [
                    'moduleId' => 'crm',
                    'id' => 7095,
                    'field' => [
                        'mandatory' => 'Y',
                        'editFormLabel' => [
                            'en' => 'List of characteristics (updated)',
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Result: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'userfieldconfig.update',
        {
            moduleId: 'crm',
            id: 7095,
            field: {
                mandatory: 'Y',
                editFormLabel: {
                    en: 'List of characteristics (updated)',
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldconfig.update',
        [
            'moduleId' => 'crm',
            'id' => 7095,
            'field' => [
                'mandatory' => 'Y',
                'editFormLabel' => [
                    'en' => 'List of characteristics (updated)',
                ],
            ],
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
    "result": {
        "field": {
            "id": "7095",
            "entityId": "CRM_7",
            "fieldName": "UF_CRM_7_NEW_REST_LIST_2026",
            "userTypeId": "enumeration",
            "xmlId": null,
            "sort": "100",
            "multiple": "Y",
            "mandatory": "Y",
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
                "en": "en"
            },
            "editFormLabel": {
                "en": "List of characteristics (updated)"
            },
            "listColumnLabel": {
                "en": null
            },
            "listFilterLabel": {
                "en": null
            },
            "errorMessage": {
                "en": null
            },
            "helpMessage": {
                "en": null
            },
            "enum": [
                {
                    "id": "3671",
                    "userFieldId": "7095",
                    "value": "Characteristic 1",
                    "def": "N",
                    "sort": "100",
                    "xmlId": "38a8c98a5de02f8ccdca2244e5065ecd"
                },
                {
                    "id": "3673",
                    "userFieldId": "7095",
                    "value": "Characteristic 2",
                    "def": "Y",
                    "sort": "200",
                    "xmlId": "9520e17b39f3525b820b809914b52207"
                }
            ]
        }
    },
    "time": {
        "start": 1774356026,
        "finish": 1774356026.949068,
        "duration": 0.9490680694580078,
        "processing": 0,
        "date_start": "2026-03-24T15:40:26+01:00",
        "date_finish": "2026-03-24T15:40:26+01:00",
        "operating_reset_at": 1774356626,
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

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **field**
[`object`](../../../data-types.md) | Settings of the updated user field [(detailed description)](#result_field) ||
|#

##### Field Object {#result_field}

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
[`boolean`](../../../data-types.md) | Multiple value flag (`Y`/`N`) ||
|| **mandatory**
[`boolean`](../../../data-types.md) | Mandatory field flag (`Y`/`N`) ||
|| **showFilter**
[`boolean`](../../../data-types.md) | Show field in filter flag ||
|| **showInList**
[`boolean`](../../../data-types.md) | Show field in list flag ||
|| **editInList**
[`boolean`](../../../data-types.md) | Edit in list flag ||
|| **isSearchable**
[`boolean`](../../../data-types.md) | Participation in search flag ||
|| **settings**
[`object`](../../../data-types.md) | Additional field settings. See [Settings Parameter](#settings).

The set of keys depends on `userTypeId` ||
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
[`object[]`](../../../data-types.md) | Value options.

The field is returned only for `userTypeId = enumeration` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "You cannot change the settings of the user field"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | You cannot change the settings of the user field | Insufficient permissions to modify the field. This same error is returned if the field with the provided `id` has already been deleted or is unavailable in the context of `moduleId` ||
|| `-` | The current method required more scopes. (crm) | The application does not have the required scope for the module from `moduleId` ||
|| `-` | No settings for UserFieldAccess | Access to user fields is not configured for the provided `moduleId` ||
|| `-` | Error while attempting to change user field settings | General error in changing the field ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldconfig-add.md)
- [{#T}](./userfieldconfig-get.md)
- [{#T}](./userfieldconfig-list.md)
- [{#T}](./userfieldconfig-delete.md)
- [{#T}](./userfieldconfig-get-types.md)
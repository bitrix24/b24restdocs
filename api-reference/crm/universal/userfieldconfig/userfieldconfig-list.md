# Get a List of User Field Settings userfieldconfig.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`userfieldconfig`](../../../scopes/permissions.md), module scope from `moduleId` (e.g., [`crm`](../../../scopes/permissions.md))
>
> Who can execute the method: a user with read access permission to the object that owns the fields in the `moduleId`

The method `userfieldconfig.list` returns a list of user field settings based on a filter.

## Method Parameters

{% include [Note on Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../../../data-types.md) | Identifier of the module in which the fields are being searched ||
|| **select**
[`object`](../../../data-types.md) | Set of fields to return [(detailed description)](#select) ||
|| **order**
[`object`](../../../data-types.md) | Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` - name of the field by which the selection will be sorted
- `value_n` - value of type `string`, equal to:
  - `ASC` - ascending sort
  - `DESC` - descending sort

Available fields for sorting:
- `id` - identifier of the user field
- `fieldName` - code of the user field
- `userTypeId` - type of the user field
- `xmlId` - external code
- `sort` - sort index

By default:
```
{
    "sort": "ASC",
    "id": "ASC"
}
```
||
|| **filter**
[`object`](../../../data-types.md) | Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` - name of the field by which the selection of user fields will be filtered
- `value_n` - filter value

All conditions for individual fields are combined using `AND`.

See below [the list of available fields for filtering](#filterable)
||
|| **start**
[`integer`](../../../data-types.md) | Offset for pagination.

Use the `next` parameter value from the previous response ||
|#

### Select Parameter {#select}

#|
|| **Name**
`type` | **Description** ||
|| **\***
[`string`](../../../data-types.md) | Return all standard settings fields ||
|| **language**
[`string`](../../../data-types.md) | Language identifier for language fields, e.g., `de` or `en` ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the field setting ||
|| **entityId**
[`string`](../../../data-types.md) | Identifier of the object ||
|| **fieldName**
[`string`](../../../data-types.md) | Code of the field ||
|| **userTypeId**
[`string`](../../../data-types.md) | Type of the field ||
|| **xmlId**
[`string`](../../../data-types.md) | External identifier ||
|| **sort**
[`string`](../../../data-types.md) | Sort index ||
|| **multiple**
[`string`](../../../data-types.md) | Is the user field multiple? Possible values: `Y` or `N` ||
|| **mandatory**
[`string`](../../../data-types.md) | Is the user field mandatory? Possible values: `Y` or `N` ||
|| **showFilter**
[`string`](../../../data-types.md) | Should the field be shown in the list filter? Possible values: `N`, `I`, `E`, `S` ||
|| **showInList**
[`string`](../../../data-types.md) | Should the field be shown in the list? Possible values: `Y` or `N` ||
|| **editInList**
[`string`](../../../data-types.md) | Is editing the value allowed in the list? Possible values: `Y` or `N` ||
|| **isSearchable**
[`string`](../../../data-types.md) | Are the field values searchable? Possible values: `Y` or `N` ||
|| **settings**
[`string`](../../../data-types.md) | Additional settings for the field ||
|| **languageId**
[`string`](../../../data-types.md) | [Language identifier](../../../data-types.md#lang-ids). When this parameter is passed, a set of language fields in the selected language is returned:
- `editFormLabel` - label in the edit form
- `listColumnLabel` - header in the list
- `listFilterLabel` - filter label in the list
- `errorMessage` - error message
- `helpMessage` - help ||
|#

### Filterable Fields {#filterable}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the user field ||
|| **fieldName**
[`string`](../../../data-types.md) | Code of the user field ||
|| **userTypeId**
[`string`](../../../data-types.md) | Type of the user field ||
|| **xmlId**
[`string`](../../../data-types.md) | External code ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **multiple**
[`boolean`](../../../data-types.md) | Is the user field multiple? Possible values: `Y` or `N` ||
|| **mandatory**
[`boolean`](../../../data-types.md) | Is the user field mandatory? Possible values: `Y` or `N` ||
|| **showFilter**
[`char`](../../../data-types.md) | Should it be shown in the list filter? Possible values: `N`, `I`, `E`, `S` ||
|| **showInList**
[`boolean`](../../../data-types.md) | Should it be shown in the list? Possible values: `Y` or `N` ||
|| **editInList**
[`boolean`](../../../data-types.md) | Is editing allowed by the user? Possible values: `Y` or `N` ||
|| **isSearchable**
[`boolean`](../../../data-types.md) | Are the field values searchable? Possible values: `Y` or `N` ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","select":{"0":"*","language":"de"},"order":{"id":"DESC"},"filter":{"multiple":"Y"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldconfig.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","select":{"0":"*","language":"de"},"order":{"id":"DESC"},"filter":{"multiple":"Y"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/userfieldconfig.list
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'userfieldconfig.list',
    		{
    			moduleId: 'crm',
    			select: {
    				0: '*',
    				language: 'de',
    			},
    			order: {
    				id: 'DESC',
    			},
    			filter: {
    				multiple: 'Y',
    			},
    			start: 0,
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
                'userfieldconfig.list',
                [
                    'moduleId' => 'crm',
                    'select' => [
                        0 => '*',
                        'language' => 'de',
                    ],
                    'order' => [
                        'id' => 'DESC',
                    ],
                    'filter' => [
                        'multiple' => 'Y',
                    ],
                    'start' => 0,
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
        'userfieldconfig.list',
        {
            moduleId: 'crm',
            select: {
                0: '*',
                language: 'de',
            },
            order: {
                id: 'DESC',
            },
            filter: {
                multiple: 'Y',
            },
            start: 0,
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
        'userfieldconfig.list',
        [
            'moduleId' => 'crm',
            'select' => [
                0 => '*',
                'language' => 'de',
            ],
            'order' => [
                'id' => 'DESC',
            ],
            'filter' => [
                'multiple' => 'Y',
            ],
            'start' => 0,
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
        "fields": [
            {
                "id": "7095",
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
                    "de": "de"
                },
                "editFormLabel": {
                    "de": "List of Characteristics"
                },
                "listColumnLabel": null,
                "listFilterLabel": null,
                "errorMessage": null,
                "helpMessage": null
            }
        ]
    },
    "next": 50,
    "total": 94,
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
|| **total**
[`integer`](../../../data-types.md) | Total number of found settings ||
|| **next**
[`integer`](../../../data-types.md) | Offset for the next page.

Field is returned if the number of found items exceeds 50 ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`object[]`](../../../data-types.md) | List of found user field settings [(detailed description)](#result_fields) ||
|#

##### Fields Object[] {#result_fields}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the user field ||
|| **entityId**
[`string`](../../../data-types.md) | Identifier of the object to which the user field belongs ||
|| **fieldName**
[`string`](../../../data-types.md) | Code of the user field ||
|| **userTypeId**
[`string`](../../../data-types.md) | Type of the user field ||
|| **xmlId**
[`string`](../../../data-types.md) | External code ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **multiple**
[`boolean`](../../../data-types.md) | Is the user field multiple? Possible values: `Y` or `N` ||
|| **mandatory**
[`boolean`](../../../data-types.md) | Is the user field mandatory? Possible values: `Y` or `N` ||
|| **showFilter**
[`char`](../../../data-types.md) | Display mode in the filter.

Possible values: `N`, `I`, `E`, `S` ||
|| **showInList**
[`boolean`](../../../data-types.md) | Should the field be shown in the list? Possible values: `Y` or `N` ||
|| **editInList**
[`boolean`](../../../data-types.md) | Is editing allowed in the list? Possible values: `Y` or `N` ||
|| **isSearchable**
[`boolean`](../../../data-types.md) | Are the field values searchable? Possible values: `Y` or `N` ||
|| **settings**
[`object`](../../../data-types.md) | Additional settings for the field.

The composition of keys depends on `userTypeId` ||
|| **languageId**
[`object`](../../../data-types.md) | Language identifiers for which labels are set ||
|| **editFormLabel**
[`lang_map`](../../../data-types.md) | Labels in the edit form ||
|| **listColumnLabel**
[`lang_map`](../../../data-types.md) | Header in the list ||
|| **listFilterLabel**
[`lang_map`](../../../data-types.md) | Filter label in the list ||
|| **errorMessage**
[`lang_map`](../../../data-types.md) | Error message ||
|| **helpMessage**
[`lang_map`](../../../data-types.md) | Help ||
|| **enum**
[`object[]`](../../../data-types.md) | List items for `userTypeId = enumeration`.

Field may be absent for other types ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "You do not have permission to view user field settings"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | You do not have permission to view user field settings | Insufficient read access permission for fields based on the provided filter ||
|| `-` | The current method required more scopes. (crm) | The application does not have the required scope for the module from `moduleId` ||
|| `-` | No settings for UserFieldAccess | Access to user fields is not configured for the provided `moduleId` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldconfig-add.md)
- [{#T}](./userfieldconfig-update.md)
- [{#T}](./userfieldconfig-get.md)
- [{#T}](./userfieldconfig-delete.md)
- [{#T}](./userfieldconfig-get-types.md)
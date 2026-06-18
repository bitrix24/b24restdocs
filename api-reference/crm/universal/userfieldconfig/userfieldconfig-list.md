# Get a List of User Field Settings userfieldconfig.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`userfieldconfig`](../../../scopes/permissions.md), module scope from `moduleId` (for example, [`crm`](../../../scopes/permissions.md))
>
> Who can execute the method: a user with read access permission to the object that owns the fields in the `moduleId`

The method `userfieldconfig.list` returns a list of user field settings based on the filter.

## Method Parameters

{% include [Note on Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../../../data-types.md) | The identifier of the module in which the fields are being searched ||
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

- `field_n` - the name of the field by which the selection will be sorted
- `value_n` - a `string` value equal to:
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

- `field_n` - the name of the field by which the selection of user fields will be filtered
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
[`string`](../../../data-types.md) | Language identifier for language fields, for example `de` or `en` ||
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
[`string`](../../../data-types.md) | Whether the user field is multiple. Possible values: `Y` or `N` ||
|| **mandatory**
[`string`](../../../data-types.md) | Whether the user field is mandatory. Possible values: `Y` or `N` ||
|| **showFilter**
[`string`](../../../data-types.md) | Whether to show the field in the list filter. Possible values: `N`, `I`, `E`, `S` ||
|| **showInList**
[`string`](../../../data-types.md) | Whether to show the field in the list. Possible values: `Y` or `N` ||
|| **editInList**
[`string`](../../../data-types.md) | Whether to allow editing the value in the list. Possible values: `Y` or `N` ||
|| **isSearchable**
[`string`](../../../data-types.md) | Whether the field values are searchable. Possible values: `Y` or `N` ||
|| **settings**
[`string`](../../../data-types.md) | Additional settings for the field ||
|| **languageId**
[`string`](../../../data-types.md) | [Language identifier](../../../data-types.md#lang-ids). When this parameter is passed, a set of language fields in the selected language is returned:
- `editFormLabel` - label in the edit form
- `listColumnLabel` - header in the list
- `listFilterLabel` - label of the filter in the list
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
[`boolean`](../../../data-types.md) | Whether the user field is multiple. Possible values: `Y` or `N` ||
|| **mandatory**
[`boolean`](../../../data-types.md) | Whether the user field is mandatory. Possible values: `Y` or `N` ||
|| **showFilter**
[`char`](../../../data-types.md) | Whether to show in the list filter. Possible values: `N`, `I`, `E`, `S` ||
|| **showInList**
[`boolean`](../../../data-types.md) | Whether to show in the list. Possible values: `Y` or `N` ||
|| **editInList**
[`boolean`](../../../data-types.md) | Whether to allow user editing. Possible values: `Y` or `N` ||
|| **isSearchable**
[`boolean`](../../../data-types.md) | Whether the field values are searchable. Possible values: `Y` or `N` ||
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type UserFieldConfig = {
      id: string
      fieldName: string
      userTypeId: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UserFieldConfigListResult = {
      fields: UserFieldConfig[]
    }

    // userfieldconfig.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<UserFieldConfigListResult>({
        method: 'userfieldconfig.list',
        params: {
          moduleId: 'crm',
          select: ['*'],
          order: {
            id: 'DESC',
          },
          filter: {
            multiple: 'Y',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(`Loaded ${result.fields.length} field config(s) on this page`)
        console.info(result.fields)
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
      async function listUserFieldConfigs() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // userfieldconfig.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'userfieldconfig.list',
            params: {
              moduleId: 'crm',
              select: ['*'],
              order: {
                id: 'DESC',
              },
              filter: {
                multiple: 'Y',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(`Loaded ${result.fields.length} field config(s) on this page`)
          console.info(result.fields)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listUserFieldConfigs)
    </script>
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
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.info(result.data());

            if (result.more()) {
                result.next();
            }
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
[`integer`](../../../data-types.md) | Total number of settings found ||
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
[`boolean`](../../../data-types.md) | Whether the user field is multiple. Possible values: `Y` or `N` ||
|| **mandatory**
[`boolean`](../../../data-types.md) | Whether the user field is mandatory. Possible values: `Y` or `N` ||
|| **showFilter**
[`char`](../../../data-types.md) | Display mode in the filter.

Possible values: `N`, `I`, `E`, `S` ||
|| **showInList**
[`boolean`](../../../data-types.md) | Whether to show the field in the list. Possible values: `Y` or `N` ||
|| **editInList**
[`boolean`](../../../data-types.md) | Whether to allow editing the value in the list. Possible values: `Y` or `N` ||
|| **isSearchable**
[`boolean`](../../../data-types.md) | Whether the field values are searchable. Possible values: `Y` or `N` ||
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
[`lang_map`](../../../data-types.md) | Label of the filter in the list ||
|| **errorMessage**
[`lang_map`](../../../data-types.md) | Error message ||
|| **helpMessage**
[`lang_map`](../../../data-types.md) | Help ||
|| **enum**
[`object[]`](../../../data-types.md) | List elements for `userTypeId = enumeration`.

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
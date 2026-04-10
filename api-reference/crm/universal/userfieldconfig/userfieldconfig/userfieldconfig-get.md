# Get User Field Settings userfieldconfig.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`userfieldconfig`](../../../../scopes/permissions.md), module scope from `moduleId` (for example, [`crm`](../../../../scopes/permissions.md))
>
> Who can execute the method: a user with read access permission to the object that owns the field in the `moduleId`

The `userfieldconfig.get` method returns the settings of a user field by its identifier.

## Method Parameters

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../../../data-types.md) | Identifier of the module where the field is located ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the user field settings.

The identifier can be obtained using the [userfieldconfig.list](./userfieldconfig-list.md) method or when creating the field with the [userfieldconfig.add](./userfieldconfig-add.md) method ||
|#

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","id":7095}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldconfig.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","id":7095,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/userfieldconfig.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'userfieldconfig.get',
    		{
    			moduleId: 'crm',
    			id: 7095,
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
                'userfieldconfig.get',
                [
                    'moduleId' => 'crm',
                    'id' => 7095,
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
        'userfieldconfig.get',
        {
            moduleId: 'crm',
            id: 7095,
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
        'userfieldconfig.get',
        [
            'moduleId' => 'crm',
            'id' => 7095,
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
                "de": "de"
            },
            "editFormLabel": {
                "en": "List of characteristics",
                "de": "Liste der Merkmale"
            },
            "listColumnLabel": {
                "en": null,
                "de": null
            },
            "listFilterLabel": {
                "en": null,
                "de": null
            },
            "errorMessage": {
                "en": null,
                "de": null
            },
            "helpMessage": {
                "en": null,
                "de": null
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
        "start": 1774355752,
        "finish": 1774355752.177393,
        "duration": 0.17739295959472656,
        "processing": 0,
        "date_start": "2026-03-24T15:35:52+01:00",
        "date_finish": "2026-03-24T15:35:52+01:00",
        "operating_reset_at": 1774356352,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. If the field is not found, it may return `null` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **field**
[`object`](../../../data-types.md) | User field settings [(detailed description)](#result_field) ||
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
[`object`](../../../data-types.md) | Additional field settings.

The composition of keys depends on `userTypeId` ||
|| **languageId**
[`object`](../../../data-types.md) | Languages for which field labels are defined ||
|| **editFormLabel**
[`lang_map`](../../../data-types.md) | Labels in the edit form ||
|| **listColumnLabel**
[`lang_map`](../../../data-types.md) | Column labels in the list ||
|| **listFilterLabel**
[`lang_map`](../../../data-types.md) | Labels in the filter ||
|| **errorMessage**
[`lang_map`](../../../data-types.md) | Error message text ||
|| **helpMessage**
[`lang_map`](../../../data-types.md) | Field help message ||
|| **enum**
[`object[]`](../../../data-types.md) | Value options.

This field is returned only for `userTypeId = enumeration` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "You do not have permission to view user field settings"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | You do not have permission to view user field settings | Insufficient rights to view the field. This error may also be returned if the field with the provided `id` has already been deleted or is unavailable in the context of `moduleId` ||
|| `-` | The current method required more scopes. (crm) | The application does not have the required scope for the module from `moduleId` ||
|| `-` | No settings for UserFieldAccess | Access to user fields is not configured for the provided `moduleId` ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldconfig-add.md)
- [{#T}](./userfieldconfig-update.md)
- [{#T}](./userfieldconfig-list.md)
- [{#T}](./userfieldconfig-delete.md)
- [{#T}](./userfieldconfig-get-types.md)
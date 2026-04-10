# Update Existing Document Template crm.documentgenerator.template.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" permission for document generator templates

The method `crm.documentgenerator.template.update` updates an existing document template.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../data-types.md) | Template identifier ||
|| **fields**^*^
[`object`](../../data-types.md) | Object with template fields to update [(more details)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Template name ||
|| **file**
[`file`](../../data-types.md#file) | Template file in the format `["file_name.docx", "base64_content"]`. Can be sent in `base64` or `multipart/form-data`. More details: [How to upload files](../../../files/how-to-upload-files.md) ||
|| **numeratorId**
[`integer`](../../data-types.md) | Identifier of the numerator. A list of available numerators can be obtained using the method [crm.documentgenerator.numerator.list](../numerator/crm-document-generator-numerator-list.md) ||
|| **region**
[`string`](../../data-types.md) | Template region, for example `de` ||
|| **entityTypeId**
[`array`](../../data-types.md) | Array of CRM entity identifiers for which the template is available.

Typical values:
- `1` — lead
- `2` — deal
- `3` — contact
- `4` — company
- `5` — invoice (old version)
- `7` — estimate
- `31` — invoice

For objects with directions, a direction suffix is added, for example:
- `2_category_0` — deal, direction `0`
- `31_1` — invoice, direction `1`

For SPAs, specify `entityTypeId` of the type from [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) or [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md), for example:
- `177` — SPA element with `entityTypeId = 177`
- `177_1` — SPA element with direction `1` ||
|| **users**
[`array`](../../data-types.md) | Array of access permission codes, for example `["UA"]`.

Supported access codes:
- `UA` — all users
- `U<id>` — user
- `G<id>` — user group
- `D<id>` — department
- `DR<id>` — department with sub-departments
- `IU<id>` — intranet user
- `SG<id>`, `SG<id>_A`, `SG<id>_E`, `SG<id>_K` — workgroup/project
- `SU<id>`, `SU<id>_M` — social network user
- `CHAT<id>` — chat
- `AD`, `AE`, `AT`, `ATD`, `ATE`, `ATT` (and variants with identifier) — role in structure
- `SND<id>`, `SNDR<id>`, `SNT<id>`, `SNTR<id>` — structural codes of departments and teams ||
|| **active**
[`char`](../../data-types.md) | Template activity: `Y` or `N` ||
|| **withStamps**
[`char`](../../data-types.md) | Include stamp and signature: `Y` or `N` ||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

Example of updating a document template where:
- template identifier — `41`
- new name — `Template from file (updated)`
- numerator identifier — `49`
- template region — `de`
- CRM entity identifier — `2`
- access permission code — `UA`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":41,"fields":{"name":"Template from file (updated)","file":["template-updated.docx","**base64_encoded_content**"],"numeratorId":49,"region":"de","entityTypeId":["2"],"users":["UA"],"active":"Y","withStamps":"N","sort":500}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.template.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":41,"fields":{"name":"Template from file (updated)","file":["template-updated.docx","**base64_encoded_content**"],"numeratorId":49,"region":"de","entityTypeId":["2"],"users":["UA"],"active":"Y","withStamps":"N","sort":500},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.template.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.template.update',
    		{
    			id: 41,
    			fields: {
    				name: 'Template from file (updated)',
    				file: ['template-updated.docx', '**base64_encoded_content**'],
    				numeratorId: 49,
    				region: 'de',
    				entityTypeId: ['2'],
    				users: ['UA'],
    				active: 'Y',
    				withStamps: 'N',
    				sort: 500,
    			},
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.documentgenerator.template.update',
                [
                    'id' => 41,
                    'fields' => [
                        'name' => 'Template from file (updated)',
                        'file' => [
                            'template-updated.docx',
                            '**base64_encoded_content**',
                        ],
                        'numeratorId' => 49,
                        'region' => 'de',
                        'entityTypeId' => ['2'],
                        'users' => ['UA'],
                        'active' => 'Y',
                        'withStamps' => 'N',
                        'sort' => 500,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.template.update',
        {
            id: 41,
            fields: {
                name: 'Template from file (updated)',
                file: ['template-updated.docx', '**base64_encoded_content**'],
                numeratorId: 49,
                region: 'de',
                entityTypeId: ['2'],
                users: ['UA'],
                active: 'Y',
                withStamps: 'N',
                sort: 500,
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
        'crm.documentgenerator.template.update',
        [
            'id' => 41,
            'fields' => [
                'name' => 'Template from file (updated)',
                'file' => [
                    'template-updated.docx',
                    '**base64_encoded_content**',
                ],
                'numeratorId' => 49,
                'region' => 'de',
                'entityTypeId' => ['2'],
                'users' => ['UA'],
                'active' => 'Y',
                'withStamps' => 'N',
                'sort' => 500,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "template": {
            "id": "41",
            "name": "Template from file (updated)",
            "region": "de",
            "code": null,
            "download": "https://mysite.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.template.download&SITE_ID=s1&id=41",
            "active": "Y",
            "moduleId": "crm",
            "numeratorId": "49",
            "withStamps": "N",
            "users": {
                "UA": "UA"
            },
            "isDeleted": "N",
            "sort": "500",
            "downloadMachine": "https://mysite.com/rest/crm.documentgenerator.template.download.json?...",
            "entityTypeId": [
                "2"
            ],
            "createTime": "2026-03-18T15:54:20+02:00",
            "updateTime": "2026-03-18T17:26:29+02:00"
        }
    },
    "time": {
        "start": 1773843989,
        "finish": 1773843989.201709,
        "duration": 0.20170903205871582,
        "processing": 0,
        "date_start": "2026-03-18T17:26:29+02:00",
        "date_finish": "2026-03-18T17:26:29+02:00",
        "operating_reset_at": 1773844589,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains the object [`template`](#template) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type template {#template}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Template identifier ||
|| **name**
[`string`](../../data-types.md) | Template name ||
|| **region**
[`string`](../../data-types.md) | Template region ||
|| **code**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Symbolic code of the template ||
|| **download**
[`string`](../../data-types.md) | Link to download the template file ||
|| **downloadMachine**
[`string`](../../data-types.md) | Link to download the template file for the application ||
|| **active**
[`char`](../../data-types.md) | Template activity: `Y` or `N` ||
|| **moduleId**
[`string`](../../data-types.md) | Module identifier ||
|| **numeratorId**
[`string`](../../data-types.md) | Identifier of the numerator ||
|| **withStamps**
[`char`](../../data-types.md) | Include stamp and signature: `Y` or `N` ||
|| **users**
[`object`](../../data-types.md) | Object of access permission codes in the format `{"UA":"UA"}` ||
|| **isDeleted**
[`char`](../../data-types.md) | Deletion status of the template: `Y` or `N` ||
|| **sort**
[`string`](../../data-types.md) | Sort index ||
|| **entityTypeId**
[`array`](../../data-types.md) | Array of CRM entity identifiers for which the template is available ||
|| **createTime**
[`datetime`](../../data-types.md) | Date and time of template creation ||
|| **updateTime**
[`datetime`](../../data-types.md) | Date and time of the last template update ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Template not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `100` | Invalid value {...} to match with parameter {fields}. Should be value of type array. | Parameter `fields` not passed as an object ||
|| `0` | Template not found | Template with the specified `id` not found or unavailable ||
|| `0` | Could not save file | Failed to save the template file ||
|| `DOCGEN_ACCESS_ERROR` | Access denied | No access to the template ||
|| `Empty value` | You do not have permissions to modify templates | Insufficient rights to modify document generator templates ||
|| `Empty value` | Module documentgenerator is not installed | Module `documentgenerator` is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-template-add.md)
- [{#T}](./crm-document-generator-template-get.md)
- [{#T}](./crm-document-generator-template-list.md)
- [{#T}](./crm-document-generator-template-delete.md)
- [{#T}](./crm-document-generator-template-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)
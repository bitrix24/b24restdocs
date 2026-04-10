# Add a New Template crm.documentgenerator.template.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for document generator templates

The method `crm.documentgenerator.template.add` adds a new document template.

## Method Parameters

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**^*^
[`object`](../../data-types.md) | Object with template parameters [(more details)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name**^*^
[`string`](../../data-types.md) | Template name ||
|| **file**^*^
[`file`](../../data-types.md#file) | Template file in the format `["file_name.docx", "base64_content"]`. Can be sent in `base64` or `multipart/form-data`. More details: [How to Upload Files](../../../files/how-to-upload-files.md) ||
|| **numeratorId**^*^
[`integer`](../../data-types.md) | Identifier of the numerator. A list of available numerators can be obtained using the method [crm.documentgenerator.numerator.list](../numerator/crm-document-generator-numerator-list.md) ||
|| **region**^*^
[`string`](../../data-types.md) | Template region, for example `de` ||
|| **entityTypeId**^*^
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
[`array`](../../data-types.md) | Array of access permission codes, for example `["UA"]`

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
- `AD`, `AE`, `AT`, `ATD`, `ATE`, `ATT` (and variants with identifier) — role in the structure
- `SND<id>`, `SNDR<id>`, `SNT<id>`, `SNTR<id>` — structural codes of departments and teams

If the parameter is not provided or is empty, the current user (`U<id>`) is automatically added to access permissions ||
|| **active**
[`char`](../../data-types.md) | Template activity: `Y` or `N`. Default is `Y` ||
|| **withStamps**
[`char`](../../data-types.md) | Include stamp and signature: `Y` or `N`. Default is `N` ||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

Example of adding a document template where:
- template name — `Estimate Template from REST`
- numerator identifier — `49`
- template region — `de`
- CRM entity identifiers — `2` and `2_category_0`
- access permission code — `UA`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Estimate Template from REST","file":["template.docx","**base64_encoded_content**"],"numeratorId":49,"region":"de","entityTypeId":["2","2_category_0"],"users":["UA"],"active":"Y","withStamps":"N","sort":500}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.template.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Estimate Template from REST","file":["template.docx","**base64_encoded_content**"],"numeratorId":49,"region":"de","entityTypeId":["2","2_category_0"],"users":["UA"],"active":"Y","withStamps":"N","sort":500},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.template.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.template.add',
    		{
    			fields: {
    				name: 'Estimate Template from REST',
    				file: ['template.docx', '**base64_encoded_content**'],
    				numeratorId: 49,
    				region: 'de',
    				entityTypeId: ['2', '2_category_0'],
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
                'crm.documentgenerator.template.add',
                [
                    'fields' => [
                        'name' => 'Estimate Template from REST',
                        'file' => [
                            'template.docx',
                            '**base64_encoded_content**',
                        ],
                        'numeratorId' => 49,
                        'region' => 'de',
                        'entityTypeId' => ['2', '2_category_0'],
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
        echo 'Error adding template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.template.add',
        {
            fields: {
                name: 'Estimate Template from REST',
                file: ['template.docx', '**base64_encoded_content**'],
                numeratorId: 49,
                region: 'de',
                entityTypeId: ['2', '2_category_0'],
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
        'crm.documentgenerator.template.add',
        [
            'fields' => [
                'name' => 'Estimate Template from REST',
                'file' => [
                    'template.docx',
                    '**base64_encoded_content**',
                ],
                'numeratorId' => 49,
                'region' => 'de',
                'entityTypeId' => ['2', '2_category_0'],
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

HTTP Status: **200**

```json
{
    "result": {
        "template": {
            "id": "41",
            "name": "Template from File",
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
            "updateTime": "2026-03-18T15:54:20+02:00"
        }
    },
    "time": {
        "start": 1773838459,
        "finish": 1773838460.813945,
        "duration": 1.8139450550079346,
        "processing": 1,
        "date_start": "2026-03-18T15:54:19+02:00",
        "date_finish": "2026-03-18T15:54:20+02:00",
        "operating_reset_at": 1773839059,
        "operating": 1.0305089950561523
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
[`char`](../../data-types.md) | Template deletion flag: `Y` or `N` ||
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

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Empty required fields: numeratorId"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `100` | Invalid value {...} to match with parameter {fields}. Should be value of type array. | Parameter `fields` not provided as an object ||
|| `0` | Empty required fields: name, numeratorId, region, entityTypeId | Required fields not provided in the `fields` object ||
|| `0` | Missing file content | Template file not provided in `fields.file` ||
|| `0` | Could not save file | Failed to save the template file ||
|| `Empty value` | You do not have permissions to modify templates | Insufficient permissions to modify document generator templates ||
|| `Empty value` | Module documentgenerator is not installed | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-template-update.md)
- [{#T}](./crm-document-generator-template-get.md)
- [{#T}](./crm-document-generator-template-list.md)
- [{#T}](./crm-document-generator-template-delete.md)
- [{#T}](./crm-document-generator-template-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)
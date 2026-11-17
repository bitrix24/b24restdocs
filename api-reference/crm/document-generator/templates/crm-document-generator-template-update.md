# Update Existing Document Template crm.documentgenerator.template.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify document generator templates and permission to modify the specified template

The method `crm.documentgenerator.template.update` updates an existing document template.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../../data-types.md) | Identifier of the template ||
|| **fields**^*^
[`object`](../../../data-types.md) | Object with template fields to update [(more details)](#fields) ||
|#

### Parameter fields {#fields}

Omitted fields retain their values. For `entityTypeId` and `users`, the list replaces the existing bindings if it is nonempty after converting identifiers or filtering access codes. Unknown `entityTypeId` identifiers are ignored; an empty array or a list that becomes empty after processing does not clear the bindings.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Template name ||
|| **file**
[`file`](../../../data-types.md) | Template file in the format `["file_name.docx", "base64_content"]`. Can be sent in `base64` or `multipart/form-data`. More details: [How to Upload Files](../../../files/how-to-upload-files.md) ||
|| **numeratorId**
[`integer`](../../../data-types.md) | Identifier of the numerator. A list of available numerators can be obtained using the method [crm.documentgenerator.numerator.list](../numerator/crm-document-generator-numerator-list.md) ||
|| **region**
[`string`](../../../data-types.md) | Template region, for example `ru` ||
|| **entityTypeId**
[`array`](../../../data-types.md) | Array of CRM object type identifiers for which the template is available.

Standard type values are listed in the [CRM Object Type](../../data-types.md#object_type) table.

For objects with pipelines, a pipeline suffix is added, for example:
- `2_category_0` — deal, pipeline `0`
- `31_1` — invoice, pipeline `1`

For SPAs, specify `entityTypeId` of the type from [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) or [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md), for example:
- `177` — SPA element with `entityTypeId = 177`
- `177_1` — SPA element with pipeline `1` ||
|| **users**
[`array`](../../../data-types.md) | Array of access permission codes, for example `["UA"]`.

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
[`char`](../../../data-types.md) | Template activity: `Y` or `N` ||
|| **withStamps**
[`char`](../../../data-types.md) | Include stamp and signature: `Y` or `N` ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting index ||
|| **timezone**
```string | null``` | Template time zone: a canonical IANA identifier, for example `Europe/Berlin`. An empty string or `null` resets the value. Used when time zone support is enabled ||
|| **titlePattern**
```string | null``` | Document title pattern. An empty string or `null` resets the value. Used when title pattern support is enabled ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of updating a document template where:
- template identifier — `41`
- new name — `Template from file (updated)`
- numerator identifier — `49`
- template region — `ru`
- CRM object type identifier — `2` (deal)
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UpdateTemplateResult = {
      template: {
        id: string
        name: string
        region: string
        code: string | null
        download: string
        downloadMachine: string
        timezone?: string | null
        titlePattern?: string | null
        active: 'Y' | 'N'
        moduleId: string
        numeratorId: string
        withStamps: 'Y' | 'N'
        users: Record<string, string> | []
        isDeleted: 'Y' | 'N'
        sort: string
        entityTypeId: string[]
        createTime: ISODate
        updateTime: ISODate
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<UpdateTemplateResult>({
        method: 'crm.documentgenerator.template.update',
        params: {
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
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.template.id, result.template.name, result.template.updateTime)
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
      async function updateTemplate() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.template.update',
            params: {
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
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.template.id, result.template.name, result.template.updateTime)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateTemplate)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.documentgenerator.template.update(
            bitrix_id=41,
            fields={
                "name": "Template from file (updated)",
                "file": ["template-updated.docx", "**base64_encoded_content**"],
                "numeratorId": 49,
                "region": "de",
                "entityTypeId": ["2"],
                "users": ["UA"],
                "active": "Y",
                "withStamps": "N",
                "sort": 500,
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.documentgenerator.template.update", b24.Params{
    	"id": 41,
    	"fields": b24.Params{
    		"name":         "Template from file (updated)",
    		"file":         []string{"template-updated.docx", "**base64_encoded_content**"},
    		"numeratorId":  49,
    		"region":       "ru",
    		"entityTypeId": []string{"2"},
    		"users":        []string{"UA"},
    		"active":       "Y",
    		"withStamps":   "N",
    		"sort":         500,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.documentgenerator.template.update: %w", err)
    }

    // The method wraps the response in an object with the "template" key.
    raw, ok := b24.Unwrap(res.Result, "template")
    if !ok {
    	return fmt.Errorf("no template key in the response")
    }

    var item struct {
    	ID       b24.ID `json:"id"`
    	Name     string `json:"name"`
    	Region   string `json:"region"`
    	Download string `json:"download"`
    	Active   string `json:"active"`
    	ModuleID string `json:"moduleId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
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
            "createTime": "2026-03-18T15:54:20+03:00",
            "updateTime": "2026-03-18T17:26:29+03:00"
        }
    },
    "time": {
        "start": 1773843989,
        "finish": 1773843989.201709,
        "duration": 0.20170903205871582,
        "processing": 0,
        "date_start": "2026-03-18T17:26:29+03:00",
        "date_finish": "2026-03-18T17:26:29+03:00",
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
[`object`](../../../data-types.md) | Root element of the response. Contains the object [`template`](#template) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Template Type {#template}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the template ||
|| **name**
[`string`](../../../data-types.md) | Template name ||
|| **region**
[`string`](../../../data-types.md) | Template region ||
|| **code**
```string | null``` | Symbolic code of the template ||
|| **download**
[`string`](../../../data-types.md) | Link to download the template file ||
|| **downloadMachine**
[`string`](../../../data-types.md) | Link to download the template file via the REST API ||
|| **active**
[`char`](../../../data-types.md) | Template activity: `Y` or `N` ||
|| **moduleId**
[`string`](../../../data-types.md) | Module identifier ||
|| **numeratorId**
[`string`](../../../data-types.md) | The identifier of the numerator ||
|| **withStamps**
[`char`](../../../data-types.md) | Include stamp and signature: `Y` or `N` ||
|| **users**
```object | array``` | Object of access permission codes in the format `{"UA":"UA"}`; an empty array `[]` if there are no codes ||
|| **isDeleted**
[`char`](../../../data-types.md) | Template deletion flag: `Y` or `N` ||
|| **sort**
[`string`](../../../data-types.md) | Sorting index ||
|| **entityTypeId**
[`array`](../../../data-types.md) | Array of CRM object type identifiers for which the template is available. The value may include a qualifier, for example `2_category_0` ||
|| **createTime**
[`datetime`](../../../data-types.md) | Date and time of template creation ||
|| **updateTime**
[`datetime`](../../../data-types.md) | Date and time of the last template update ||
|| **timezone**
```string | null``` | Template time zone. Returned when time zone support is enabled ||
|| **titlePattern**
```string | null``` | Document title pattern. Returned when title pattern support is enabled ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Template not found"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `TITLE_PATTERN_INVALID` | Title pattern must be a string | The value passed in `titlePattern` is not a string ||
|| `400` | `TITLE_PATTERN_INVALID` | Invalid document title pattern. Check the placeholders and make sure the pattern is no longer than 255 characters | The `titlePattern` value failed placeholder or length validation ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `400` | `100` | Invalid value {...} to match with parameter {fields}. Should be value of type array. | Parameter `fields` not provided as an object ||
|| `400` | `0` | Template was not found | Template with the specified `id` not found ||
|| `400` | `0` | Could not save file | Failed to save the template file ||
|| `400` | `0` | You do not have permissions to modify templates | Insufficient permissions to modify document generator templates ||
|| `400` | `0` | Module documentgenerator is not installed | The `documentgenerator` module is not available ||
|| `400` | `0` | You do not have permissions to modify this template | Insufficient permissions to modify the specified template ||
|| `400` | `TIMEZONE_INVALID` | Timezone must be a canonical IANA identifier. | An invalid time zone identifier was passed in `timezone` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-template-add.md)
- [{#T}](./crm-document-generator-template-get.md)
- [{#T}](./crm-document-generator-template-list.md)
- [{#T}](./crm-document-generator-template-delete.md)
- [{#T}](./crm-document-generator-template-get-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)

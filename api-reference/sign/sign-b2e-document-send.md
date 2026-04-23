# Send Document for Signing sign.b2e.document.send

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sign.b2e`](../scopes/permissions.md), [`crm`](../scopes/permissions.md)
>
> Who can execute the method: a user with permission to create documents in KEDO

The method `sign.b2e.document.send` sends a document for signing on behalf of the company.

This method works only in the context of application authorization [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields*** 
[`object`](../data-types.md) | Parameters for sending the document for signing [(detailed description)](#fields) ||
|| **language**
[`string`](../data-types.md) | Language for localizing statuses in the response.

Defaults to `en` ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **company*** 
[`object`](../data-types.md) | The company on behalf of which the document is sent [(detailed description)](#company) ||
|| **members*** 
[`array`](../data-types.md) | List of signing participants. Must contain at least one participant with the role `signer` and one with the role `assignee` [(detailed description)](#members) ||
|| **responsible*** 
[`object`](../data-types.md) | Responsible person for the document [(detailed description)](#responsible) ||
|| **companyProviderUid*** 
[`string`](../data-types.md) | Identifier of the signing provider. You can get a list of available signing providers using the [sign.b2e.company.provider.list](./sign-b2e-company-provider-list.md) method ||
|| **files*** 
[`array`](../data-types.md) | File of the document being signed [(detailed description)](#files) ||
|| **regionDocumentType*** 
[`string`](../data-types.md) | Document type for the region [(detailed description)](#region-document-type) ||
|| **externalSettings*** 
[`object`](../data-types.md) | External parameters of the document [(detailed description)](#external-settings) ||
|| **language**
[`string`](../data-types.md) | Language of the document ||
|#

### Parameter company {#company}

#|
|| **Name**
`type` | **Description** ||
|| **uuid*** 
[`string`](../data-types.md) | UUID of the company in HCM Link.

Requires additional scope [`humanresources.hcmlink`](../scopes/permissions.md) ||
|| **crmId*** 
[`integer`](../data-types.md) | Identifier of the company in CRM, connected in the integration as "my company" ||
|#

Pass either the `uuid` or `crmId` parameter.

### Array Element members {#members}

#|
|| **Name**
`type` | **Description** ||
|| **employeeCode*** 
[`string`](../data-types.md) | Employee code in HCM Link.

Requires additional scope [`humanresources.hcmlink`](../scopes/permissions.md) ||
|| **employeeId*** 
[`integer`](../data-types.md) | Employee identifier in HCM Link.

Requires additional scope [`humanresources.hcmlink`](../scopes/permissions.md) ||
|| **userId*** 
[`integer`](../data-types.md) | User identifier in Bitrix24 ||
|| **role*** 
[`string`](../data-types.md) | Role of the participant. Possible values: 
- `signer` — signer, 
- `assignee` — company representative, 
- `reviewer` — approver, 
- `editor` — editor ||
|#

Pass either the `employeeId`, `employeeCode`, or `userId` parameter.

The `employeeCode` and `employeeId` fields are available only for companies linked to HCM Link. For companies passed through `company.crmId` without a connection to HCM Link, use `userId`.

### Parameter responsible {#responsible}

#|
|| **Name**
`type` | **Description** ||
|| **employeeCode*** 
[`string`](../data-types.md) | Employee code in HCM Link.

Requires additional scope [`humanresources.hcmlink`](../scopes/permissions.md) ||
|| **employeeId*** 
[`integer`](../data-types.md) | Employee identifier in HCM Link.

Requires additional scope [`humanresources.hcmlink`](../scopes/permissions.md) ||
|| **userId*** 
[`integer`](../data-types.md) | User identifier in Bitrix24 ||
|| **role**
[`string`](../data-types.md) | Role of the participant. Possible values:
- `signer` — signer, 
- `assignee` — company representative, default value
- `reviewer` — approver, 
- `editor` — editor ||
|#

Pass either the `employeeId`, `employeeCode`, or `userId` parameter.

The `employeeCode` and `employeeId` fields are available only for companies linked to HCM Link. For companies passed through `company.crmId` without a connection to HCM Link, use `userId`.

### Array Element files {#files}

#|
|| **Name**
`type` | **Description** ||
|| **fileName*** 
[`string`](../data-types.md) | File name. Must include the `.pdf` extension ||
|| **fileType*** 
[`string`](../data-types.md) | MIME type of the file. Supported `application/pdf` ||
|| **fileContent*** 
[`string`](../data-types.md) | Content of the file, encoded in Base64 ||
|#

### Parameter regionDocumentType {#region-document-type}

The document type code depends on the license region. The list of available codes is formed on the Bitrix24 side, and validation is performed based on the portal's region.

If you do not know the code, use the value `12.999` as a safe default. This value passes validation by default when the list of codes for the region is empty or unknown.

### Parameter externalSettings {#external-settings}

#|
|| **Name**
`type` | **Description** ||
|| **externalId*** 
[`string`](../data-types.md) | External identifier. Maximum length — 255 characters ||
|| **externalDateCreate*** 
[`string`](../data-types.md) | Creation date of the external document in ISO 8601 format ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"company":{"crmId":12},"members":[{"userId":25,"role":"signer"},{"userId":42,"role":"assignee"}],"responsible":{"userId":7},"companyProviderUid":"d4f6b8a1-4c6d-4d8c-9c7c-2d1b1f6d0f2b","files":[{"fileName":"contract.pdf","fileType":"application/pdf","fileContent":"JVBERi0xLjQKJ..."}],"regionDocumentType":"12.999","externalSettings":{"externalId":"EXT-123","externalDateCreate":"2025-02-18T09:19:34+01:00"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sign.b2e.document.send
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sign.b2e.document.send',
    		{
    			fields: {
    				company: { crmId: 12 },
    				members: [
    					{ userId: 25, role: 'signer' },
    					{ userId: 42, role: 'assignee' }
    				],
    				responsible: { userId: 7 },
    				companyProviderUid: 'd4f6b8a1-4c6d-4d8c-9c7c-2d1b1f6d0f2b',
    				files: [
    					{
    						fileName: 'contract.pdf',
    						fileType: 'application/pdf',
    						fileContent: 'JVBERi0xLjQKJ...'
    					}
    				],
    				regionDocumentType: '12.999',
    				externalSettings: {
    					externalId: 'EXT-123',
    					externalDateCreate: '2025-02-18T09:19:34+01:00'
    				}
    			}
    		}
    	);
    
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
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
                'sign.b2e.document.send',
                [
                    'fields' => [
                        'company' => [
                            'crmId' => 12
                        ],
                        'members' => [
                            [ 'userId' => 25, 'role' => 'signer' ],
                            [ 'userId' => 42, 'role' => 'assignee' ]
                        ],
                        'responsible' => [ 'userId' => 7 ],
                        'companyProviderUid' => 'd4f6b8a1-4c6d-4d8c-9c7c-2d1b1f6d0f2b',
                        'files' => [
                            [
                                'fileName' => 'contract.pdf',
                                'fileType' => 'application/pdf',
                                'fileContent' => 'JVBERi0xLjQKJ...'
                            ]
                        ],
                        'regionDocumentType' => '12.999',
                        'externalSettings' => [
                            'externalId' => 'EXT-123',
                            'externalDateCreate' => '2025-02-18T09:19:34+01:00'
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'sign.b2e.document.send',
        {
            fields: {
                company: { crmId: 12 },
                members: [
                    { userId: 25, role: 'signer' },
                    { userId: 42, role: 'assignee' }
                ],
                responsible: { userId: 7 },
                companyProviderUid: 'd4f6b8a1-4c6d-4d8c-9c7c-2d1b1f6d0f2b',
                files: [
                    {
                        fileName: 'contract.pdf',
                        fileType: 'application/pdf',
                        fileContent: 'JVBERi0xLjQKJ...'
                    }
                ],
                regionDocumentType: '12.999',
                externalSettings: {
                    externalId: 'EXT-123',
                    externalDateCreate: '2025-02-18T09:19:34+01:00'
                }
            }
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.document.send',
        [
            'fields' => [
                'company' => [ 'crmId' => 12 ],
                'members' => [
                    [ 'userId' => 25, 'role' => 'signer' ],
                    [ 'userId' => 42, 'role' => 'assignee' ]
                ],
                'responsible' => [ 'userId' => 7 ],
                'companyProviderUid' => 'd4f6b8a1-4c6d-4d8c-9c7c-2d1b1f6d0f2b',
                'files' => [
                    [
                        'fileName' => 'contract.pdf',
                        'fileType' => 'application/pdf',
                        'fileContent' => 'JVBERi0xLjQKJ...'
                    ]
                ],
                'regionDocumentType' => '12.999',
                'externalSettings' => [
                    'externalId' => 'EXT-123',
                    'externalDateCreate' => '2025-02-18T09:19:34+01:00'
                ]
            ]
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "uid": "b6f5f1f1-9d20-4b6b-ae0f-2f0a8a0c2b3c",
        "state": {
            "code": "sent",
            "name": "Sent"
        },
        "members": [
            {
                "uid": "f1c2d3e4",
                "role": "signer",
                "party": 0,
                "user": {
                    "employeeCode": "EMP-001",
                    "employeeId": 123,
                    "userId": 25
                },
                "state": {
                    "code": "waiting",
                    "name": "Waiting"
                }
            }
        ]
    },
    "time": {
        "start": 1739860000.123,
        "finish": 1739860000.456,
        "duration": 0.333,
        "processing": 0.111,
        "date_start": "2025-02-18T09:19:34+01:00",
        "date_finish": "2025-02-18T09:19:34+01:00",
        "operating_reset_at": 1739860600,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Information about the document and signing participants ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result object

#|
|| **Name**
`type` | **Description** ||
|| **uid**
[`string`](../data-types.md) | Unique identifier of the document ||
|| **state**
[`object`](../data-types.md) | Current status of the document ||
|| **members**
[`array`](../data-types.md) | Signing participants ||
|#

#### Fields of the result.state object

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../data-types.md) | Status code ||
|| **name**
[`string`](../data-types.md) | Status name ||
|#

#### Element of the result.members array

#|
|| **Name**
`type` | **Description** ||
|| **uid**
[`string`](../data-types.md) | Unique identifier of the participant ||
|| **role**
[`string`](../data-types.md) | Role of the participant ||
|| **party**
[`integer`](../data-types.md) | Signing party ||
|| **user**
[`object`](../data-types.md) | User data ||
|| **state**
[`object`](../data-types.md) | Status of the participant ||
|#

#### Fields of the result.members.user object

#|
|| **Name**
`type` | **Description** ||
|| **employeeCode**
[`string`](../data-types.md) | Employee code in HCM Link.

Returned only for companies linked to HCM Link ||
|| **employeeId**
[`integer`](../data-types.md) | Employee identifier in HCM Link.

Returned only for companies linked to HCM Link ||
|| **userId**
[`integer`](../data-types.md) | User identifier in Bitrix24 ||
|#

#### Fields of the result.members.state object

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../data-types.md) | Status code of the participant ||
|| **name**
[`string`](../data-types.md) | Name of the participant's status ||
|#

## Error Handling

HTTP Status: **200**

```json
{
    "error": "BAD_REQUEST",
    "error_description": "[companyProviderUid] field is required"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **When it occurs** ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions ||
|| `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | Call not from application context ||
|| `BAD_REQUEST` | Empty request fields | `fields` object not provided ||
|| `BAD_REQUEST` | companyProviderUid field is required | `companyProviderUid` not provided ||
|| `BAD_REQUEST` | The responsible field is required | `responsible` not provided ||
|| `BAD_REQUEST` | At least one signing party with role signer is required | No participant with role `signer` in `members` ||
|| `BAD_REQUEST` | At least one signing party with role assignee is required | No participant with role `assignee` in `members` ||
|| `BAD_REQUEST` | Invalid role ... | Role provided outside of allowed values ||
|| `BAD_REQUEST` | Employee ID is required | `employeeId`/`employeeCode` not provided and `userId` not provided ||
|| `BAD_REQUEST` | Employee with code ... was not found | Employee with specified `employeeCode` not found ||
|| `BAD_REQUEST` | Employee with ID ... was not found | Employee with specified `employeeId` not found ||
|| `BAD_REQUEST` | Could not find employee for specified User ID | User by `userId` not found ||
|| `BAD_REQUEST` | Could not find user for specified employee | User not found for employee ||
|| `BAD_REQUEST` | At least one file is required to sign the document | `files` array not provided ||
|| `BAD_REQUEST` | Signing multiple files is currently not supported | More than one file provided in `files` ||
|| `BAD_REQUEST` | The specified file type is not supported | `fileType` other than `application/pdf` specified ||
|| `BAD_REQUEST` | Invalid file name | Invalid file name ||
|| `BAD_REQUEST` | Invalid file name. File extension is not specified | File extension not specified ||
|| `BAD_REQUEST` | The specified file extension is not supported | File extension not supported ||
|| `BAD_REQUEST` | Region document type is required. Use value '12.999' as a default value | `regionDocumentType` not provided when codes list is available ||
|| `BAD_REQUEST` | Region document type is not valid. Use value '12.999' as a default value | Unsupported `regionDocumentType` provided ||
|| `BAD_REQUEST` | Signing provider is not available | Provider not available for the company ||
|| `BAD_REQUEST` | Signing provider has expired | Signing provider has expired ||
|| `BAD_REQUEST` | External date create field is required | `externalSettings.externalDateCreate` not provided ||
|| `BAD_REQUEST` | External ID field is required | `externalSettings.externalId` not provided ||
|| `BAD_REQUEST` | External date create invalid field value | Invalid date format in `externalSettings.externalDateCreate` ||
|| `BAD_REQUEST` | External ID maximum length of 255 is exceeded | `externalSettings.externalId` length exceeded ||
|| `INTERNAL_ERROR` | Internal error | Error sending document or generating response ||
|| `-` | humanresources module is not installed | `humanresources` module not installed when using HCM Link fields ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-document-get.md)
- [{#T}](./sign-b2e-company-provider-list.md)
- [{#T}](./index.md)
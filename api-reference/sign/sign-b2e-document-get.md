# Get Document sign.b2e.document.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sign.b2e`](../scopes/permissions.md), [`crm`](../scopes/permissions.md)
>
> Who can execute the method: a user with document viewing access permission in KEDO

The method `sign.b2e.document.get` returns information about the document and signing participants.

The method works only in the context of application authorization [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **uid**
[`string`](../data-types.md) | Unique identifier of the document ||
|| **language**
[`string`](../data-types.md) | Language for localizing statuses in the response.

Defaults to `en` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"uid":"b6f5f1f1-9d20-4b6b-ae0f-2f0a8a0c2b3c","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sign.b2e.document.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sign.b2e.document.get',
    		{
    			uid: 'b6f5f1f1-9d20-4b6b-ae0f-2f0a8a0c2b3c',
    			language: 'en'
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
                'sign.b2e.document.get',
                [
                    'uid' => 'b6f5f1f1-9d20-4b6b-ae0f-2f0a8a0c2b3c',
                    'language' => 'en'
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
        'sign.b2e.document.get',
        {
            uid: 'b6f5f1f1-9d20-4b6b-ae0f-2f0a8a0c2b3c',
            language: 'en'
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
        'sign.b2e.document.get',
        [
            'uid' => 'b6f5f1f1-9d20-4b6b-ae0f-2f0a8a0c2b3c',
            'language' => 'en'
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
            "code": "in_progress",
            "name": "In Progress"
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
                    "code": "signed",
                    "name": "Signed"
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

Returned only for companies linked with HCM Link ||
|| **employeeId**
[`integer`](../data-types.md) | Employee identifier in HCM Link.

Returned only for companies linked with HCM Link ||
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
[`string`](../data-types.md) | Status name of the participant ||
|#

## Error Handling

HTTP Status: **200**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **When it occurs** ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions ||
|| `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | Call not from application context ||
|| `INTERNAL_ERROR` | Internal error | Error generating response ||
|| `-` | Document UID is required | Parameter `uid` not provided ||
|| `-` | Document not found | Document with the specified `uid` not found ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-document-send.md)
- [{#T}](./sign-b2e-company-provider-list.md)
- [{#T}](./index.md)
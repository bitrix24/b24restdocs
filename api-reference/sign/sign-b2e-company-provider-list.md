# Get the List of Providers for sign.b2e.company.provider.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sign.b2e`](../scopes/permissions.md), [`crm`](../scopes/permissions.md)
>
> Who can execute the method: a user with permission to create KEDO documents

The method `sign.b2e.company.provider.list` returns a list of signature providers for the selected company.

The method works only in the context of application authorization [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **companyUuid*** 
[`string`](../data-types.md) | UUID of the company in HCM Link.

Required if the `companyCrmId` parameter is not provided.

Requires additional scope [`humanresources.hcmlink`](../scopes/permissions.md) ||
|| **companyCrmId*** 
[`integer`](../data-types.md) | Identifier of the company in CRM, connected in the integration as "my company".

Required if the `companyUuid` parameter is not provided ||
|| **language** 
[`string`](../data-types.md) | Language for localizing provider names.

Defaults to `en` ||
|| **limit** 
[`integer`](../data-types.md) | Number of records per page.

The parameter accepts values from 1 to 1000.

Defaults to 100 ||
|| **offset** 
[`integer`](../data-types.md) | Parameter for managing pagination.

Defaults to 0 ||
|#

You must provide either the `companyUuid` or `companyCrmId` parameter.

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"companyCrmId":12,"limit":2,"offset":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sign.b2e.company.provider.list
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sign.b2e.company.provider.list',
            {
                // UUID of the company in HCM Link or ID of the company in CRM
                companyCrmId: 12,

                // Language for localizing provider names
                language: 'de',

                // Pagination parameters
                limit: 2,
                offset: 0
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
                'sign.b2e.company.provider.list',
                [
                    'companyCrmId' => 12,
                    'language' => 'de',
                    'limit' => 2,
                    'offset' => 0
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
        'sign.b2e.company.provider.list',
        {
            companyCrmId: 12,
            language: 'de',
            limit: 2,
            offset: 0
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
        'sign.b2e.company.provider.list',
        [
            'companyCrmId' => 12,
            'language' => 'de',
            'limit' => 2,
            'offset' => 0
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
    "result": [
        {
            "code": "ses-de",
            "uid": "d4f6b8a1-4c6d-4d8c-9c7c-2d1b1f6d0f2b",
            "name": "Bitrix24 KEDO",
            "date": "2025-02-18T10:15:30+01:00",
            "expires": "2026-02-18T10:15:30+01:00"
        }
    ],
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
[`array`](../data-types.md) | List of signature providers for the company ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Element of the result array

#| 
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../data-types.md) | Provider code ||
|| **uid**
[`string`](../data-types.md) | Unique identifier of the provider ||
|| **name**
[`string`](../data-types.md) | Name of the provider considering the language ||
|| **date**
[`string`](../data-types.md) | Registration date of the provider in ISO 8601 format ||
|| **expires**
[`string`](../data-types.md) | Expiration date of the provider in ISO 8601 format ||
|#

## Error Handling

HTTP Status: **200**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **When it occurs** ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions ||
|| `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | Call not from application context ||
|| `510` | Company was not found. | Company not found by `companyUuid` or `companyCrmId` ||
|| `510` | My company ... not found | CRM company not found ||
|| `-` | Parameter 'companyUuid' or 'companyCrmId' is required | Company identifiers not provided ||
|| `-` | humanresources module is not installed | `humanresources` module not installed when using `companyUuid` ||
|| `-` | Error while autoregistering providers | Error in auto-registering virtual providers. Check my company data in CRM ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-document-send.md)
- [{#T}](./sign-b2e-document-get.md)
- [{#T}](./index.md)
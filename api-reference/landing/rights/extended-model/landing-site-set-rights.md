# Set Access Permissions for landing.site.setRights

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator or user with "full access" permission to the "Sites and Stores" section

The method `landing.site.setRights` sets access permissions in the advanced permission model for the specified site.

This method only works in the advanced permission model. If the role model is enabled in the "Sites and Stores" section, the call will return `true`, but the saved permissions will not be applied. To enable the advanced permission model, use the method [landing.role.enable](../landing-role-enable.md) with the value `mode: 0`.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) \| [`string`](../../../data-types.md) | Site identifier.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) or from the result of the method [landing.site.add](../../site/landing-site-add.md).

The special value `0` allows saving a separate set of permissions. Individual permissions for a specific site are set by its `id` ||
|| **rights**
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | Object format:

```json
{
    "access_code_1": ["operation_1", "operation_2"],
    "access_code_2": ["operation_1"]
}
```

where:
- `access_code_n` — access code
- `operation_n` — operation code

The list of access codes and operations is described [below](#rights). The method completely replaces previously saved individual permissions for the site.

If the parameter is not passed, an empty object `{}` or an empty array `[]` is passed, the method will clear the individual permissions for the site ||
|#

### Parameter rights {#rights}

#|
|| **Name**
`type` | **Description** ||
|| **<ACCESS_CODE>**
[`string[]`](../../../data-types.md) | List of operations for a single access code.

Possible values:
`denied` - deny access
`read` - allow viewing the site
`edit` - allow editing the site pages
`sett` - allow changing site settings
`public` - allow publishing the site
`delete` - allow moving the site to the trash and restoring it

If `denied` is present in the set, all other operations for this access code are ignored.

If `read` is not present in the set and `denied` is absent, the method will automatically add `read`. Unknown operation codes are ignored without error ||
|#

Use Bitrix24 access codes as keys for the `rights` object. Common options include:

- `U<ID>` - user
- `G<ID>` - user group
- `DR<ID>` - department along with sub-departments
- `UA` - all users, including guests
- `AU` - all authorized users
- `SG<ID>` - working group

If access is needed only for authorized users, use `AU`. The code `UA` opens access to all users, including guests. The method does not check the format of the access code when saving. If an unsupported code is passed, the request will complete without error, but such a code will not provide working access.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 645,
        "rights": {
          "AU": ["read"],
          "U3": ["read", "edit", "sett", "public"]
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.setRights.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 645,
        "rights": {
          "AU": ["read"],
          "U3": ["read", "edit", "sett", "public"]
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.setRights.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.site.setRights',
            {
                id: 645,
                rights: {
                    AU: ['read'],
                    U3: ['read', 'edit', 'sett', 'public']
                }
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
                'landing.site.setRights',
                [
                    'id' => 645,
                    'rights' => [
                        'AU' => ['read'],
                        'U3' => ['read', 'edit', 'sett', 'public'],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting site rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.setRights',
        {
            id: 645,
            rights: {
                AU: ['read'],
                U3: ['read', 'edit', 'sett', 'public']
            }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.site.setRights',
        [
            'id' => 645,
            'rights' => [
                'AU' => ['read'],
                'U3' => ['read', 'edit', 'sett', 'public'],
            ],
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1775055086,
        "finish": 1775055086.8533,
        "duration": 0.8533000946044922,
        "processing": 0,
        "date_start": "2026-04-01T17:51:26+02:00",
        "date_finish": "2026-04-01T17:51:26+02:00",
        "operating_reset_at": 1775055686,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of saving permissions.

- `true` — permissions successfully saved or cleared
- `false` — site with such `id` not found or already in the trash

The method does not return the final list of permissions.

After the call, check the applied permissions using the method [landing.site.getRights](./landing-site-get-rights.md) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: id"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | User does not have access to the "Sites and Stores" section ||
|| `IS_NOT_ADMIN` | Administrator rights or "full access" permission to the "Sites and Stores" section are required for the method ||
|| `FEATURE_NOT_AVAIL` | Permission settings are not available on the current plan. To work with permissions, switch to another plan ||
|| `MISSING_PARAMS` | Required parameter `id` not passed ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-get-rights.md)
- [{#T}](../landing-role-is-enabled.md)
- [{#T}](../landing-role-enable.md)
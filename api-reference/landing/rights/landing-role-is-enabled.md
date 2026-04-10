# Check if the role model of permissions is enabled: landing.role.isEnabled

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "full access" permission to the "Sites and Stores" section

The method `landing.role.isEnabled` checks whether the role model of permissions is enabled for the "Sites and Stores" section.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{}' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.role.isEnabled.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.role.isEnabled.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.role.isEnabled',
            {}
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
                'landing.role.isEnabled',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error checking role model: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.role.isEnabled',
        {},
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
        'landing.role.isEnabled',
        []
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
        "start": 1774988172,
        "finish": 1774988172.261264,
        "duration": 0.2612640857696533,
        "processing": 0,
        "date_start": "2026-03-31T23:16:12+01:00",
        "date_finish": "2026-03-31T23:16:12+01:00",
        "operating_reset_at": 1774988772,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Indicates whether the role model of permissions is enabled.

`true` - the role model of permissions is in use. To configure permissions, use [landing.role.getList](./role-model/landing-role-get-list.md), [landing.role.getRights](./role-model/landing-role-get-rights.md), [landing.role.setAccessCodes](./role-model/landing-role-set-access-codes.md), and [landing.role.setRights](./role-model/landing-role-set-rights.md)

`false` - the extended model of permissions is in use. To configure permissions for a specific site, use [landing.site.getRights](./extended-model/landing-site-get-rights.md) and [landing.site.setRights](./extended-model/landing-site-set-rights.md) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "IS_NOT_ADMIN",
    "error_description": "To perform this action, you must be an administrator."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions to access the "Sites and Stores" section ||
|| `IS_NOT_ADMIN` | The method is available only to users with "full access" permission to the "Sites and Stores" section ||
|| `FEATURE_NOT_AVAIL` | Permission management in the "Sites and Stores" section is not available on the current plan ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-role-enable.md)
- [{#T}](./extended-model/landing-site-get-rights.md)
- [{#T}](./extended-model/landing-site-set-rights.md)
- [{#T}](./role-model/landing-role-get-list.md)
- [{#T}](./role-model/landing-role-get-rights.md)
- [{#T}](./role-model/landing-role-set-access-codes.md)
- [{#T}](./role-model/landing-role-set-rights.md)
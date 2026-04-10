# Get Role Rights with landing.role.getRights

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator or user with "full access" permission to the "Sites and Stores" section.

The method `landing.role.getRights` returns the rights of the specified role for each site where they are configured.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Role identifier. You can obtain the identifier using the [landing.role.getList](./landing-role-get-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 2
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.role.getRights.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 2,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.role.getRights.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.role.getRights',
            {
                id: 2
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
                'landing.role.getRights',
                [
                    'id' => 2,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting role rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.role.getRights',
        {
            id: 2
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
        'landing.role.getRights',
        [
            'id' => 2,
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
    "result": {
        "0": [
            "read",
            "edit"
        ],
        "5": [
            "read"
        ]
    },
    "time": {
        "start": 1775064046,
        "finish": 1775064046.935469,
        "duration": 0.9354689121246338,
        "processing": 0,
        "date_start": "2026-04-01T20:20:46+02:00",
        "date_finish": "2026-04-01T20:20:46+02:00",
        "operating_reset_at": 1775064646,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | Array of role rights by sites, where the key is the site identifier and the value is an array of rights codes [(detailed description)](#result-item).

If the role has no saved rights or a role with such `id` is not found, the method returns an empty array `[]` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Element {#result-item}

#|
|| **Name**
`type` | **Description** ||
|| **`0`**
[`string[]`](../../../data-types.md) | Default role rights for all sites that do not have separate settings.

Available values are described [below](#right-codes) ||
|| **`<siteId>`**
[`string[]`](../../../data-types.md) | Role rights for the site with the specified identifier.

The site identifier can be obtained using the [landing.site.getList](../../site/landing-site-get-list.md) method or from the result of the [landing.site.add](../../site/landing-site-add.md) method.

Available values are described [below](#right-codes) ||
|#

#### Rights Codes {#right-codes}

#|
|| **Code** | **Description** ||
|| `denied` | Access to the site is denied ||
|| `read` | Read ||
|| `edit` | Modify site pages ||
|| `sett` | Change site settings ||
|| `public` | Publish ||
|| `delete` | Move to trash and restore from trash ||
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
|| `ACCESS_DENIED` | No access to the "Sites and Stores" section ||
|| `IS_NOT_ADMIN` | Administrator rights or "full access" permission to the "Sites and Stores" section are required for the method ||
|| `FEATURE_NOT_AVAIL` | Rights management in the "Sites and Stores" section is not available on the current plan ||
|| `MISSING_PARAMS` | Required parameter `id` is missing ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-role-set-rights.md)
- [{#T}](./landing-role-get-list.md)
- [{#T}](./landing-role-set-access-codes.md)
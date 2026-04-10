# Get the List of Roles landing.role.getList

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator or user with "full access" permission to the "Sites and Stores" section

The method `landing.role.getList` retrieves a list of access roles for the selected type of sites.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The type of sites for which roles need to be retrieved. This parameter is not related to the REST scope `landing` in the method name.

The values `GROUP`, `KNOWLEDGE`, and `MAINPAGE` correspond to the types of sites described in the article [Working with Site Types and Scopes](../../types.md).

Possible values:
`GROUP` - roles for group sites
`KNOWLEDGE` - roles for knowledge bases
`MAINPAGE` - roles for the main page or vibe

By default, the method returns roles for sites and online stores. This will be the case if the parameter is not provided or an unsupported value is specified ||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "scope": "KNOWLEDGE"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.role.getList.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "scope": "KNOWLEDGE",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.role.getList.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.role.getList',
            {
                scope: 'KNOWLEDGE'
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
                'landing.role.getList',
                [
                    'scope' => 'KNOWLEDGE',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting role list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.role.getList',
        {
            scope: 'KNOWLEDGE'
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
        'landing.role.getList',
        [
            'scope' => 'KNOWLEDGE',
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
    "result": [
        {
            "ID": "3",
            "TITLE": "Administrator",
            "XML_ID": "ADMIN"
        },
        {
            "ID": "5",
            "TITLE": "Manager",
            "XML_ID": "MANAGER"
        }
    ],
    "time": {
        "start": 1775062049,
        "finish": 1775062049.634052,
        "duration": 0.634052038192749,
        "processing": 0,
        "date_start": "2026-04-01T19:47:29+01:00",
        "date_finish": "2026-04-01T19:47:29+01:00",
        "operating_reset_at": 1775062649,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | A list of roles for the selected type of sites [(detailed description)](#role).

If no roles are found, the method returns `result: []` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Role Object {#role}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../../data-types.md) | The identifier of the role. Use it in the methods [landing.role.getRights](./landing-role-get-rights.md), [landing.role.setAccessCodes](./landing-role-set-access-codes.md), and [landing.role.setRights](./landing-role-set-rights.md) ||
|| **TITLE**
[`string`](../../../data-types.md) | The name of the role in the interface ||
|| **XML_ID**
[`string`](../../../data-types.md) | The system code of the role.

Possible values for automatically created standard roles:
`ADMIN` - administrator
`MANAGER` - manager ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "IS_NOT_ADMIN",
    "error_description": "To perform this action, you must be an administrator."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | No authorization or insufficient rights to work with the "Sites and Stores" module ||
|| `IS_NOT_ADMIN` | The method requires administrator rights or "full access" permission to the "Sites and Stores" section ||
|| `FEATURE_NOT_AVAIL` | Management of permissions in the "Sites and Stores" section is not available on the current plan ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-role-get-rights.md)
- [{#T}](./landing-role-set-access-codes.md)
- [{#T}](./landing-role-set-rights.md)
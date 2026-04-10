# Set Access Codes for Role landing.role.setAccessCodes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator or user with "full access" permission to the "Sites and Stores" section.

The method `landing.role.setAccessCodes` specifies to whom the role is assigned: users, groups, or departments. After invocation, the method reapplies the already saved permissions of this role for the sites.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Role identifier. You can obtain the identifier using the method [landing.role.getList](./landing-role-get-list.md) ||
|| **codes**
[`string[]`](../../../data-types.md) | The final list of access codes for the role.

The method completely replaces the previously saved list and does not merge it with the current one.

Access code options:
- `U<ID>` — user
- `G<ID>` — user group
- `DR<ID>` — department along with sub-departments
- `AU` — all authorized users
- `SG<ID>` — working group

More details about access codes and their usage rules can be found in the description of the method [landing.site.setRights](../extended-model/landing-site-set-rights.md).

The method does not check each access code individually. If there is an unsupported or non-existent code in the list, there will be no separate error.

If the `codes` parameter is not provided, the role's code list will be cleared. However, the saved role permissions for the sites do not automatically disappear, so after invocation, access may remain for more users than expected.

After changing the access codes, the system recalculates not only the permissions for the sites but also the additional permissions of the role: the ability to create sites, view the "Sites and Stores" section in the menu, and administer the section.

You cannot retrieve the saved list of access codes via REST. The method [landing.role.getList](./landing-role-get-list.md) returns only the identifier, name, and `XML_ID` of the role, while [landing.role.getRights](./landing-role-get-rights.md) shows only the role's permissions for the sites.

If the `codes` parameter is passed in a format other than an array, the method will return an `ERROR_ARGUMENT` error. ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 11,
        "codes": [
          "U45",
          "DR7",
          "SG3_A"
        ]
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.role.setAccessCodes.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 11,
        "codes": [
          "U45",
          "DR7",
          "SG3_A"
        ],
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.role.setAccessCodes.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.role.setAccessCodes',
            {
                id: 11,
                codes: [
                    'U45',
                    'DR7',
                    'SG3_A'
                ]
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
                'landing.role.setAccessCodes',
                [
                    'id' => 11,
                    'codes' => [
                        'U45',
                        'DR7',
                        'SG3_A',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting role access codes: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.role.setAccessCodes',
        {
            id: 11,
            codes: [
                'U45',
                'DR7',
                'SG3_A'
            ]
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
        'landing.role.setAccessCodes',
        [
            'id' => 11,
            'codes' => [
                'U45',
                'DR7',
                'SG3_A',
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

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1775067129,
        "finish": 1775067129.196438,
        "duration": 0.19643807411193848,
        "processing": 0,
        "date_start": "2026-04-01T21:12:09+02:00",
        "date_finish": "2026-04-01T21:12:09+02:00",
        "operating_reset_at": 1775067729,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Call result

The method returns `true` if the request completed without access or system errors.

The value `true` alone does not confirm that a role with such `id` exists or that the list of codes was changed.

After invocation, check the result in the interface. You can additionally verify which permissions of the role are applied to the sites via the method [landing.role.getRights](./landing-role-get-rights.md), but this method does not return the final list of access codes. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

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
|| `ACCESS_DENIED` | Insufficient permissions to work with the "Sites and Stores" section. ||
|| `IS_NOT_ADMIN` | The method requires administrator rights or "full access" permission to the "Sites and Stores" section. ||
|| `FEATURE_NOT_AVAIL` | Managing permissions in the "Sites and Stores" section is not available on the current plan. ||
|| `MISSING_PARAMS` | The required parameter `id` is missing. ||
|| `ERROR_ARGUMENT` | The `codes` parameter is not passed in array format. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-role-get-list.md)
- [{#T}](./landing-role-get-rights.md)
- [{#T}](./landing-role-set-rights.md)
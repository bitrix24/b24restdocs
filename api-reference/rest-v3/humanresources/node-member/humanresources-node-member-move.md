# Move Participants to the humanresources.node.member.move Department

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Add Employees to Department" or "Add Participants to Team" access permission.

The method `humanresources.node.member.move` transfers users to the specified department or team.

## Method Parameters

{% include [Parameters Note](../../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **nodeId*** 
[`integer`](../../../data-types.md) | Identifier of the target department or team.

The identifier can be obtained using the [humanresources.node.list](../node/humanresources-node-list.md) method. ||
|| **userIds*** 
[`array`](../../../data-types.md) | Array of user identifiers to be transferred.

User identifiers can be obtained using the [user.get](../../../user/user-get.md) method. ||
|| **role** 
[`string`](../../../data-types.md) | Role to be assigned to the transferred participants.

Possible values for department:

- `MEMBER_HEAD` — department head
- `MEMBER_DEPUTY_HEAD` — deputy head of the department
- `MEMBER_EMPLOYEE` — department employee

Possible values for team:

- `MEMBER_TEAM_HEAD` — team leader
- `MEMBER_TEAM_DEPUTY_HEAD` — deputy team leader
- `MEMBER_TEAM_EMPLOYEE` — team member 

Default: `MEMBER_EMPLOYEE` for department and `MEMBER_TEAM_EMPLOYEE` for team. ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.member.move`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"nodeId":28,"userIds":[12,18],"role":"MEMBER_EMPLOYEE"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.member.move
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"nodeId":28,"userIds":[12,18],"role":"MEMBER_EMPLOYEE","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.member.move
    ```

- JS

    The SDK does not currently support calls to the /rest/api/ address. Use direct HTTP requests, such as via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'humanresources.node.member.move',
            {
                nodeId: 28,
                userIds: [12, 18],
                role: 'MEMBER_EMPLOYEE'
            }
        );

        const result = response.getData().result;
        console.log('Member move result:', result.success);
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not currently support calls to the /rest/api/ address. Use direct HTTP requests, such as via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.member.move',
                [
                    'nodeId' => 28,
                    'userIds' => [12, 18],
                    'role' => 'MEMBER_EMPLOYEE',
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

    The SDK does not currently support calls to the /rest/api/ address. Use direct HTTP requests, such as via curl or fetch.

    ```js
    BX24.callMethod(
        'humanresources.node.member.move',
        {
            nodeId: 28,
            userIds: [12, 18],
            role: 'MEMBER_EMPLOYEE'
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not currently support calls to the /rest/api/ address. Use direct HTTP requests, such as via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.node.member.move',
        [
            'nodeId' => 28,
            'userIds' => [12, 18],
            'role' => 'MEMBER_EMPLOYEE',
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
        "success": true
    },
    "time": {
        "start": 1780406100,
        "finish": 1780406100.215497,
        "duration": 0.21549701690673828,
        "processing": 0.1711289882659912,
        "date_start": "2026-06-02T16:15:00+02:00",
        "date_finish": "2026-06-02T16:15:00+02:00",
        "operating_reset_at": 1780406700,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name** 
`type` | **Description** ||
|| **result** 
[`object`](../../../data-types.md) | Object containing the operation result. ||
|| **success** 
[`boolean`](../../../data-types.md) | Value `true` if users were successfully transferred. ||
|| **time** 
[`time`](../../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Record with ID = `28` not found."
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `nodeId` | Parameter `"nodeId"` is required. | Provide the identifier of the department or team. ||
|| `userIds` | Parameter `"userIds"` is required and must be a non-empty array. | Provide a non-empty array of user identifiers. ||
|| `role` | Invalid role `#ROLE#`. Allowed: `#ROLE_LIST#`. | Provide a role that is supported for the selected type of department or team. ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied. | Ensure the user has permission to add participants to the target department or team. ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient access rights: required scope is missing. | Check the `humanresources` scope. ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `nodeId` | Record with the specified identifier not found. | Provide the identifier of an existing department or team. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-member-add.md)
- [{#T}](./humanresources-node-member-remove.md)
- [{#T}](./index.md)

<!-- Generated by skill-1-gen-docs from source: C:\Users\g.m.tagirova\github\b24-rest-docs\api-reference\rest-v3\OpenAPI.md; C:\Users\g.m.tagirova\original-bitrix\modules\humanresources\lib\Rest\Controller\Node\Member.php -->
<!-- Generated: 2026-06-02 -->
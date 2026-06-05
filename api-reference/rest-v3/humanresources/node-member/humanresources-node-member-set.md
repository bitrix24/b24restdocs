# Update the Composition of Participants in humanresources.node.member.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Add Employees to Department" or "Add Participants to Team" access permission.

The method `humanresources.node.member.set` updates the composition of participants in a department or team by roles.

## Method Parameters

{% include [Note on Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **nodeId***
[`integer`](../../../data-types.md) | Identifier of the department or team.

The identifier can be obtained using the [humanresources.node.list](../node/humanresources-node-list.md) method. ||
|| **userIds***
[`object`](../../../data-types.md) | An object containing lists of user identifiers in the format `{"User_Role": [User_Identifier]}`. [Description of the object structure](#userids)

The method adds new participants, updates roles of existing ones, and removes users from the department who are not present in the input `userIds` object.

User identifiers can be obtained using the [user.get](../../../user/user-get.md) method. ||
|#

### Parameter userIds {#userids}

#|
|| **Name**
`type` | **Description** ||
|| **MEMBER_HEAD**
[`array`](../../../data-types.md) | Identifiers of department heads. ||
|| **MEMBER_DEPUTY_HEAD**
[`array`](../../../data-types.md) | Identifiers of deputy department heads. ||
|| **MEMBER_EMPLOYEE**
[`array`](../../../data-types.md) | Identifiers of department employees. ||
|| **MEMBER_TEAM_HEAD**
[`array`](../../../data-types.md) | Identifiers of team leaders. ||
|| **MEMBER_TEAM_DEPUTY_HEAD**
[`array`](../../../data-types.md) | Identifiers of deputy team leaders. ||
|| **MEMBER_TEAM_EMPLOYEE**
[`array`](../../../data-types.md) | Identifiers of team participants. ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by the addition of the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.member.set`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"nodeId":15,"userIds":{"MEMBER_HEAD":[7],"MEMBER_DEPUTY_HEAD":[12],"MEMBER_EMPLOYEE":[18,25,31]}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.member.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"nodeId":15,"userIds":{"MEMBER_HEAD":[7],"MEMBER_DEPUTY_HEAD":[12],"MEMBER_EMPLOYEE":[18,25,31]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.member.set
    ```

- JS

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'humanresources.node.member.set',
            {
                nodeId: 15,
                userIds: {
                    MEMBER_HEAD: [7],
                    MEMBER_DEPUTY_HEAD: [12],
                    MEMBER_EMPLOYEE: [18, 25, 31]
                }
            }
        );

        const result = response.getData().result;
        console.log('Member set result:', result.success);
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.member.set',
                [
                    'nodeId' => 15,
                    'userIds' => [
                        'MEMBER_HEAD' => [7],
                        'MEMBER_DEPUTY_HEAD' => [12],
                        'MEMBER_EMPLOYEE' => [18, 25, 31],
                    ],
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

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'humanresources.node.member.set',
        {
            nodeId: 15,
            userIds: {
                MEMBER_HEAD: [7],
                MEMBER_DEPUTY_HEAD: [12],
                MEMBER_EMPLOYEE: [18, 25, 31]
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.node.member.set',
        [
            'nodeId' => 15,
            'userIds' => [
                'MEMBER_HEAD' => [7],
                'MEMBER_DEPUTY_HEAD' => [12],
                'MEMBER_EMPLOYEE' => [18, 25, 31]
            ]
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
        "start": 1780405900,
        "finish": 1780405900.242662,
        "duration": 0.2426619529724121,
        "processing": 0.19870305061340332,
        "date_start": "2026-06-02T16:11:40+02:00",
        "date_finish": "2026-06-02T16:11:40+02:00",
        "operating_reset_at": 1780406500,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing the result of the operation. ||
|| **success**
[`boolean`](../../../data-types.md) | Value `true` if the composition of participants was successfully updated. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "Invalid roles: MEMBER_OWNER. Allowed: MEMBER_HEAD, MEMBER_DEPUTY_HEAD, MEMBER_EMPLOYEE.",
                "field": "userIds"
            }
        ]
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
|| `userIds` | Parameter `"userIds"` is required and must be a non-empty array. | Provide an object with non-empty user lists by roles. ||
|| `userIds` | Invalid roles: `#ROLE#`. Allowed: `#ROLE_LIST#`. | Provide only roles that are supported for the selected type of department or team. ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied. | Check that the user has permission to modify the composition of participants in this department or team. ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient permissions: required scope is missing. | Check the `humanresources` scope. ||
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
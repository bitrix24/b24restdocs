# Add Participants to humanresources.node.member.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Add Employees to Department" or "Add Participants to Team" permission.

The method `humanresources.node.member.add` adds users to a department or team.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **nodeId*** 
[`integer`](../../../data-types.md) | Identifier of the department or team.

The identifier can be obtained using the [humanresources.node.list](../node/humanresources-node-list.md) method. ||
|| **userIds*** 
[`array`](../../../data-types.md) | An array of user identifiers to be added to the department or team. If a user is already part of this department or team, the method will update their role.

User identifiers can be obtained using the [user.get](../../../user/user-get.md) method. ||
|| **role*** 
[`string`](../../../data-types.md) | The role to be assigned to all users in `userIds`.

Possible values for the department:

- `MEMBER_HEAD` — department head
- `MEMBER_DEPUTY_HEAD` — deputy head of the department
- `MEMBER_EMPLOYEE` — department employee

Possible values for the team:

- `MEMBER_TEAM_HEAD` — team leader
- `MEMBER_TEAM_DEPUTY_HEAD` — deputy team leader
- `MEMBER_TEAM_EMPLOYEE` — team member ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.member.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"nodeId":15,"userIds":[7,12,18],"role":"MEMBER_EMPLOYEE"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.member.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"nodeId":15,"userIds":[7,12,18],"role":"MEMBER_EMPLOYEE","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.member.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NodeMemberAddResult = {
      success: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<NodeMemberAddResult>({
        method: 'humanresources.node.member.add',
        params: {
          nodeId: 15,
          userIds: [7, 12, 18],
          role: 'MEMBER_EMPLOYEE',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Members added successfully:', result.success)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addNodeMembers() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.node.member.add',
            params: {
              nodeId: 15,
              userIds: [7, 12, 18],
              role: 'MEMBER_EMPLOYEE',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Members added successfully:', result.success)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addNodeMembers)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.member.add',
                [
                    'nodeId' => 15,
                    'userIds' => [7, 12, 18],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'humanresources.node.member.add',
        {
            nodeId: 15,
            userIds: [7, 12, 18],
            role: 'MEMBER_EMPLOYEE'
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.node.member.add',
        [
            'nodeId' => 15,
            'userIds' => [7, 12, 18],
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
        "start": 1780405600,
        "finish": 1780405600.184422,
        "duration": 0.18442201614379883,
        "processing": 0.12311410903930664,
        "date_start": "2026-06-02T16:06:40+02:00",
        "date_finish": "2026-06-02T16:06:40+02:00",
        "operating_reset_at": 1780406200,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name** 
`type` | **Description** ||
|| **result** 
[`object`](../../../data-types.md) | Object containing the result of the operation ||
|| **success** 
[`boolean`](../../../data-types.md) | Value `true` if users were successfully added to the department or team ||
|| **time** 
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
                "message": "Parameter \"role\" is required.",
                "field": "role"
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
|| `nodeId` | Parameter `"nodeId"` is required. | Provide the identifier of the department or team in the `nodeId` parameter. ||
|| `userIds` | Parameter `"userIds"` is required and must be a non-empty array. | Provide a non-empty array of user identifiers. ||
|| `role` | Parameter `"role"` is required. | Provide the participant's role in the `role` parameter. ||
|| `role` | Role `#ROLE#` not found. | Provide an existing role for the selected type of department or team. ||
|| `role` | Invalid role `#ROLE#`. Allowed: `#ROLE_LIST#`. | Provide a role that is supported for the selected type of department or team. ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Check that the user has permission to add participants to this department or team. ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient permissions: required scope is missing | Check the `humanresources` scope. ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `nodeId` | Record with the specified identifier not found | Provide the identifier of an existing department or team. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-member-set.md)
- [{#T}](./humanresources-node-member-move.md)
- [{#T}](./index.md)

# Remove Participants from the Department humanresources.node.member.remove

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Remove employees from the department" or "Remove participants from the team" access permission.

The method `humanresources.node.member.remove` removes users from the specified department or team.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **nodeId***
[`integer`](../../../data-types.md) | Identifier of the department or team.

The identifier can be obtained using the [humanresources.node.list](../node/humanresources-node-list.md) method. ||
|| **userIds***
[`array`](../../../data-types.md) | Array of user identifiers to be removed.

User identifiers can be obtained using the [user.get](../../../user/user-get.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.member.remove`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"nodeId":15,"userIds":[18,25,31]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.member.remove
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"nodeId":15,"userIds":[18,25,31],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.member.remove
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type RemoveMemberResult = {
      removed: number[]
      failed: {
        userId: number
        reason: string
      }[]
    }

    try {
      const response = await $b24.actions.v3.call.make<RemoveMemberResult>({
        method: 'humanresources.node.member.remove',
        params: {
          nodeId: 15,
          userIds: [18, 25, 31],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Removed:', result.removed)
        console.info('Failed:', result.failed)
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
      async function removeMember() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.node.member.remove',
            params: {
              nodeId: 15,
              userIds: [18, 25, 31],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Removed:', result.removed)
          console.info('Failed:', result.failed)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', removeMember)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.member.remove',
                [
                    'nodeId' => 15,
                    'userIds' => [18, 25, 31],
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
        'humanresources.node.member.remove',
        {
            nodeId: 15,
            userIds: [18, 25, 31]
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
        'humanresources.node.member.remove',
        [
            'nodeId' => 15,
            'userIds' => [18, 25, 31],
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
        "removed": [
            18,
            25
        ],
        "failed": [
            {
                "userId": 31,
                "reason": "Not found in this node"
            }
        ]
    },
    "time": {
        "start": 1780406400,
        "finish": 1780406400.207548,
        "duration": 0.2075481414794922,
        "processing": 0.16550898551940918,
        "date_start": "2026-06-02T16:20:00+02:00",
        "date_finish": "2026-06-02T16:20:00+02:00",
        "operating_reset_at": 1780407000,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing the operation result ||
|| **removed**
[`array`](../../../data-types.md) | Array of user identifiers that were successfully removed ||
|| **failed**
[`array`](../../../data-types.md) | Array of users that could not be removed ||
|| **failed[]**
[`object`](../../../data-types.md) | Object describing the removal error ||
|| **userId**
[`integer`](../../../data-types.md) | Identifier of the user that could not be removed ||
|| **reason**
[`string`](../../../data-types.md) | Reason why the user was not removed ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating the request object",
        "validation": [
            {
                "message": "Parameter \"userIds\" is required and must be a non-empty array.",
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
|| `userIds` | Parameter `"userIds"` is required and must be a non-empty array. | Provide a non-empty array of user identifiers. ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Ensure the user has permission to remove participants from this department or team. ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient access rights: required scope is missing | Check the `humanresources` scope. ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `nodeId` | Record with the specified identifier not found | Provide the identifier of an existing department or team. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-member-add.md)
- [{#T}](./humanresources-node-member-move.md)
- [{#T}](./index.md)

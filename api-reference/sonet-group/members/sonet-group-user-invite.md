# Invite users to group sonet_group.user.invite

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can perform the method: a user with the permission to Invite to a group or project

The method `sonet_group.user.invite` sends invitations to users in a workgroup or project.

## Method parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **GROUP_ID***
[`integer`](../../data-types.md) | Identifier of the workgroup or project.

The identifier can be obtained using the [sonet_group.get](../sonet-group-get.md) method. ||
|| **USER_ID***
[`integer/array`](../../data-types.md) | User identifier.

The identifier can be obtained using the [user.get](../../user/user-get.md) method. ||
|| **MESSAGE**
[`string`](../../data-types.md) | Invitation text. ||
|#

## Code examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":69,"USER_ID":1271,"MESSAGE":"Join the project"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.user.invite
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":69,"USER_ID":1271,"MESSAGE":"Join the project","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.user.invite
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<string[]>({
        method: 'sonet_group.user.invite',
        params: {
          GROUP_ID: 69,
          USER_ID: 1271,
          MESSAGE: 'Join the project',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Invited user IDs:', result)
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
      async function inviteGroupUser() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sonet_group.user.invite',
            params: {
              GROUP_ID: 69,
              USER_ID: 1271,
              MESSAGE: 'Join the project',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Invited user IDs:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', inviteGroupUser)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sonet_group.user.invite',
                [
                    'GROUP_ID' => 69,
                    'USER_ID' => 1271,
                    'MESSAGE' => 'Join the project'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error inviting user to group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sonet_group.user.invite',
        {
            GROUP_ID: 69,
            USER_ID: 1271,
            MESSAGE: 'Join the project'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sonet_group.user.invite',
        [
            'GROUP_ID' => 69,
            'USER_ID' => 1271,
            'MESSAGE' => 'Join the project'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response handling

HTTP status: **200**

```json
{
    "result": ["1271"],
    "time": {
        "start": 1773846932,
        "finish": 1773846933.077318,
        "duration": 1.0773179531097412,
        "processing": 0,
        "date_start": "2026-03-18T18:15:32+02:00",
        "date_finish": "2026-03-18T18:15:33+02:00",
        "operating_reset_at": 1773847533,
        "operating": 0
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array of user identifiers that were successfully invited.

An empty array means that none of the provided users could be invited. For example, the user is already a member of the group or the current user does not have permission to invite. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Wrong user IDs"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong group ID` | An incorrect `GROUP_ID` was provided. ||
|| — | `Wrong user IDs` | An empty or incorrect `USER_ID` was provided. ||
|| — | `Socialnetwork group not found` | The group or project was not found or is not accessible to the current user. ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue exploring

- [{#T}](./sonet-group-user-request.md)
- [{#T}](./sonet-group-user-add.md)
- [{#T}](./sonet-group-user-update.md)
- [{#T}](./sonet-group-user-get.md)
- [{#T}](./sonet-group-user-delete.md)
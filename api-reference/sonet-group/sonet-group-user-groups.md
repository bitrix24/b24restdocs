# Get the list of groups for the current user sonet_group.user.groups

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `sonet_group.user.groups` returns the groups and projects that the current user is a member of.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.user.groups
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.user.groups
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each UserGroupItem returned in result[]
    type UserGroupItem = {
      GROUP_ID: string
      GROUP_NAME: string
      ROLE: string
      GROUP_IMAGE_ID: string | null
      GROUP_IMAGE: string
      IS_EXTRANET?: string
    }

    try {
      // sonet_group.user.groups returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<UserGroupItem[]>({
        method: 'sonet_group.user.groups',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User groups count:', result.length, result)
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
      async function getUserGroups() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sonet_group.user.groups returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sonet_group.user.groups',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('User groups count:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getUserGroups)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sonet_group.user.groups',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving user groups: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sonet_group.user.groups',
        {}, 
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
        'sonet_group.user.groups',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
        "GROUP_ID": "77",
        "GROUP_NAME": "New Project Title",
        "ROLE": "K",
        "GROUP_IMAGE_ID": null,
        "GROUP_IMAGE": ""
        },
        {
        "GROUP_ID": "79",
        "GROUP_NAME": "Scrum Project",
        "ROLE": "A",
        "GROUP_IMAGE_ID": null,
        "GROUP_IMAGE": ""
        }
    ],
    "time": {
        "start": 1773927027,
        "finish": 1773927028.025164,
        "duration": 1.0251638889312744,
        "processing": 1,
        "date_start": "2026-03-19T16:30:27+02:00",
        "date_finish": "2026-03-19T16:30:28+02:00",
        "operating_reset_at": 1773927627,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Array of groups the current user belongs to.

An empty array means that the user is not a member of any group or project ||
|| **GROUP_ID**
[`integer`](../data-types.md) | Group identifier ||
|| **GROUP_NAME**
[`string`](../data-types.md) | Group name ||
|| **ROLE**
[`string`](../data-types.md) | Role of the current user.

Possible values:
- `A` — owner
- `E` — moderator
- `K` — participant ||
|| **GROUP_IMAGE_ID**
[`string`](../data-types.md) | Group avatar identifier ||
|| **GROUP_IMAGE**
[`string`](../data-types.md) | URL of the group avatar ||
|| **IS_EXTRANET**
[`string`](../data-types.md) | Indicator of an extranet group.

This field is returned only for extranet groups:
- `Y` — extranet group ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-get.md)
- [{#T}](./socialnetwork-api-workgroup-list.md)
- [{#T}](./socialnetwork-api-workgroup-get.md)
- [{#T}](./members/index.md)
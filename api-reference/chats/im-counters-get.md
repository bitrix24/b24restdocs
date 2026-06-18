# Get Counters im.counters.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.counters.get` retrieves the counters for unread messages and notifications for the current user.

## Method Parameters

No parameters.

## Code Examples

{% include [Example Notes](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.counters.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.counters.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CountersResult = {
      TYPE: {
        ALL: number
        NOTIFY: number
        CHAT: number
        LINES: number
        DIALOG: number
        MESSENGER: number
      }
      CHAT: Record<string, number>
      CHAT_MUTED: Record<string, number>
      CHAT_UNREAD: number[]
      LINES: Record<string, number>
      DIALOG: Record<string, number>
      DIALOG_UNREAD: number[]
    }

    try {
      const response = await $b24.actions.v2.call.make<CountersResult>({
        method: 'im.counters.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(
          'Total unread:', result.TYPE.ALL,
          'Notifications:', result.TYPE.NOTIFY,
          'Chats:', result.TYPE.CHAT,
          'Dialogs:', result.TYPE.DIALOG
        )
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
      async function getCounters() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.counters.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(
            'Total unread:', result.TYPE.ALL,
            'Notifications:', result.TYPE.NOTIFY,
            'Chats:', result.TYPE.CHAT,
            'Dialogs:', result.TYPE.DIALOG
          )
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCounters)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call('im.counters.get', []);

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

    ```js
    BX24.callMethod(
        'im.counters.get',
        {},
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
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

    $result = CRest::call('im.counters.get', []);

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
        "TYPE": {
            "ALL": 5,
            "NOTIFY": 4,
            "CHAT": 0,
            "LINES": 0,
            "DIALOG": 1,
            "MESSENGER": 1
        },
        "CHAT": [],
        "CHAT_MUTED": {
            "1317": 1,
            "1319": 1
        },
        "CHAT_UNREAD": [],
        "LINES": [],
        "DIALOG": {
            "547": 1
        },
        "DIALOG_UNREAD": []
    },
    "time": {
        "start": 1772010465,
        "finish": 1772010465.508917,
        "duration": 0.5089170932769775,
        "processing": 0,
        "date_start": "2026-02-25T12:07:45+01:00",
        "date_finish": "2026-02-25T12:07:45+01:00",
        "operating_reset_at": 1772011065,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root object with counters ||
|| **TYPE**
[`object`](../data-types.md) | Total counters by type [(detailed description)](#type) ||
|| **CHAT**
[`array`](../data-types.md) | Counters for chats. Key — chat ID, value — number of unread messages ||
|| **CHAT_MUTED**
[`object`](../data-types.md) | Counters for chats with notifications disabled. Key — chat ID, value — number of unread messages ||
|| **CHAT_UNREAD**
[`array`](../data-types.md) | Counters for chats marked as "Unread" ||
|| **LINES**
[`array`](../data-types.md) | Counters for open line chats ||
|| **DIALOG**
[`object`](../data-types.md) | Counters for one-on-one dialogs. Key — user ID, value — number of unread messages ||
|| **DIALOG_UNREAD**
[`array`](../data-types.md) | Counters for one-on-one dialogs marked as "Unread" ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### TYPE Object {#type}

#|
|| **Name**
`type` | **Description** ||
|| **ALL**
[`integer`](../data-types.md) | Total number of unread messages and notifications ||
|| **CHAT**
[`integer`](../data-types.md) | Total number of unread messages in chats ||
|| **DIALOG**
[`integer`](../data-types.md) | Total number of unread messages in dialogs ||
|| **LINES**
[`integer`](../data-types.md) | Total number of unread messages in open lines ||
|| **NOTIFY**
[`integer`](../data-types.md) | Total number of unread notifications ||
|| **MESSENGER**
[`integer`](../data-types.md) | Total number of unread messages in the messenger (excluding notifications) ||
|#

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-recent-get.md)
- [{#T}](./im-recent-list.md)
- [{#T}](./im-dialog-get.md)
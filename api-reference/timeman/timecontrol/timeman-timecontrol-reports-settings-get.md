# Get Report Settings timeman.timecontrol.reports.settings.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.timecontrol.reports.settings.get` retrieves report settings for building the report interface of the time control tool.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.timecontrol.reports.settings.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.timecontrol.reports.settings.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TimeControlReportsSettingsResult = {
      active: boolean
      user_id: number
      user_admin: boolean
      user_head: boolean
      departments: { id: string; name: string }[]
      minimum_idle_for_report: number
      report_view_type: 'none' | 'head' | 'full' | 'simple'
    }

    try {
      const response = await $b24.actions.v2.call.make<TimeControlReportsSettingsResult>({
        method: 'timeman.timecontrol.reports.settings.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(
          'active:', result.active,
          'user_id:', result.user_id,
          'report_view_type:', result.report_view_type,
          'departments:', result.departments
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
      async function getTimeControlReportsSettings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'timeman.timecontrol.reports.settings.get',
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
            'active:', result.active,
            'user_id:', result.user_id,
            'report_view_type:', result.report_view_type,
            'departments:', result.departments
          )
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTimeControlReportsSettings)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.timecontrol.reports.settings.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting time control reports settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'timeman.timecontrol.reports.settings.get',
        {},
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.timecontrol.reports.settings.get',
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
    "result": {
        "active": true,
        "user_id": 547,
        "user_admin": false,
        "user_head": true,
        "departments": [
            {
                "id": "9",
                "name": "Marketing and Advertising Department"
            }
        ],
        "minimum_idle_for_report": 15,
        "report_view_type": "head"
    },
    "time": {
        "start": 1749211142.210397,
        "finish": 1749211142.280698,
        "duration": 0.07030105590820312,
        "processing": 0.03640604019165039,
        "date_start": "2025-06-06T14:59:02+02:00",
        "date_finish": "2025-06-06T14:59:02+02:00",
        "operating_reset_at": 1749211742,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **active**
[`boolean`](../../data-types.md) | Is the time control module active? ||
|| **user_id**
[`integer`](../../data-types.md) | Identifier of the current user ||
|| **user_admin**
[`boolean`](../../data-types.md) | Is the user an administrator? ||
|| **user_head**
[`boolean`](../../data-types.md) | Is the user a manager? ||
|| **departments**
[`array`](../../data-types.md) | List of objects with information about [departments](#departments). The content of the array depends on the user's role:
- Administrator — description of all departments,
- Manager — description of their department,
- Regular employee — empty array ||
|| **minimum_idle_for_report**
[`integer`](../../data-types.md) | Minimum idle time in minutes after which a report is required ||
|| **report_view_type**
[`string`](../../data-types.md) | Type of report view. Possible values:
- `none` — no access to reports
- `head` — manager
- `full` — full access
- `simple` — simplified access ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Departments Object {#departments}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Identifier of the department ||
- || **name**
[`string`](../../data-types.md) | Name of the department ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-timecontrol-report-add.md)
- [{#T}](./timeman-timecontrol-reports-get.md)
- [{#T}](./timeman-timecontrol-reports-users-get.md)

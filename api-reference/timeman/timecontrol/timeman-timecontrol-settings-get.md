# Get Time Control Settings timeman.timecontrol.settings.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `timeman.timecontrol.settings.get` retrieves the settings of the time control module.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.timecontrol.settings.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.timecontrol.settings.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TimecontrolSettingsGetResult = {
      active: boolean
      minimum_idle_for_report: number
      register_offline: boolean
      register_idle: boolean
      register_desktop: boolean
      report_request_type: string
      report_request_users: number[]
      report_simple_type: string
      report_simple_users: number[]
      report_full_type: string
      report_full_users: number[]
    }

    try {
      const response = await $b24.actions.v2.call.make<TimecontrolSettingsGetResult>({
        method: 'timeman.timecontrol.settings.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('active:', result.active, 'report_request_type:', result.report_request_type)
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
      async function getTimecontrolSettings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'timeman.timecontrol.settings.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('active:', result.active, 'report_request_type:', result.report_request_type)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTimecontrolSettings)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.timecontrol.settings.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting time control settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'timeman.timecontrol.settings.get',
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
        'timeman.timecontrol.settings.get',
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
        "minimum_idle_for_report": 15,
        "register_offline": true,
        "register_idle": true,
        "register_desktop": true,
        "report_request_type": "all",
        "report_request_users": [],
        "report_simple_type": "all",
        "report_simple_users": [],
        "report_full_type": "all",
        "report_full_users": []
    },
    "time": {
        "start": 1748526089.625516,
        "finish": 1748526089.656787,
        "duration": 0.03127098083496094,
        "processing": 0.008746147155761719,
        "date_start": "2025-05-29T16:41:29+02:00",
        "date_finish": "2025-05-29T16:41:29+02:00",
        "operating_reset_at": 1748526689,
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
[`boolean`](../../data-types.md) | Activity of the time control module ||
|| **minimum_idle_for_report**
[`integer`](../../data-types.md) | Minimum idle time in minutes after which a report is required ||
|| **register_offline**
[`boolean`](../../data-types.md) | Register offline status ||
|| **register_idle**
[`boolean`](../../data-types.md) | Register away status ||
|| **register_desktop**
[`boolean`](../../data-types.md) | Register desktop application status ||
|| **report_request_type**
[`string`](../../data-types.md) | Type of report requests. Possible values:
- `all` — for everyone
- `user` — for specific users
- `none` — for no one ||
|| **report_request_users**
[`array`](../../data-types.md) | Array of user IDs for whom report requests are required.

Filled if `report_request_type` is set to `user` ||
|| **report_simple_type**
[`string`](../../data-types.md) | Type of simple report. Possible values:
- `all` — for everyone
- `user` — for specific users
- `none` — for no one ||
|| **report_simple_users**
[`array`](../../data-types.md) | Array of user IDs with access to the simple report.

Filled if `report_simple_type` is set to `user` ||
|| **report_full_type**
[`string`](../../data-types.md) | Type of full report. Possible values:
- `all` — for everyone
- `user` — for specific users
- `none` — for no one ||
|| **report_full_users**
[`array`](../../data-types.md) | Array of user IDs with access to the full report.

Filled if `report_full_type` is set to `user` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to use this method"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_ERROR` | You don't have access to use this method | You do not have access to this method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-timecontrol-report-add.md)
- [{#T}](./timeman-timecontrol-reports-get.md)
- [{#T}](./timeman-timecontrol-reports-users-get.md)

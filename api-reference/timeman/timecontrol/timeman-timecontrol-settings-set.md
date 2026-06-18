# Set Time Control Settings timeman.timecontrol.settings.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `timeman.timecontrol.settings.set` sets the configurations for the time control module.

{% note warning "" %}

Data collection for time control begins once the corresponding registration settings are activated. It is not possible to retrieve data for the period prior to activation — the system does not retain historical information retrospectively.

{% endnote %}

## Method Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **ACTIVE**
[`boolean`](../../data-types.md) | Activate the time control module. Possible values:
- `true` or `1` — enable the module
- `0` — disable the module. The option `'ACTIVE': false` does not disable the module
||
|| **MINIMUM_IDLE_FOR_REPORT**
[`integer`](../../data-types.md) | Minimum idle time in minutes after which a report is required ||
|| **REGISTER_OFFLINE**
[`boolean`](../../data-types.md) | Register offline status ||
|| **REGISTER_IDLE**
[`boolean`](../../data-types.md) | Register idle status ||
|| **REGISTER_DESKTOP**
[`boolean`](../../data-types.md) | Register desktop application status ||
|| **REPORT_REQUEST_TYPE**
[`string`](../../data-types.md) | Type of report requests. Possible values:
- `all` — for everyone
- `user` — for specific users
- `none` — for no one ||
|| **REPORT_REQUEST_USERS**
[`array`](../../data-types.md) | Array of user IDs for whom report requests are required.

Filled if `REPORT_REQUEST_TYPE` is set to `user` ||
|| **REPORT_SIMPLE_TYPE**
[`string`](../../data-types.md) | Type of simple report. Possible values:
- `all` — for everyone
- `user` — for specific users
- `none` — for no one ||
|| **REPORT_SIMPLE_USERS**
[`array`](../../data-types.md) | Array of user IDs with access to the simple report.

Filled if `REPORT_SIMPLE_TYPE` is set to `user` ||
|| **REPORT_FULL_TYPE**
[`string`](../../data-types.md) | Type of full report. Possible values:
- `all` — for everyone
- `user` — for specific users
- `none` — for no one ||
|| **REPORT_FULL_USERS**
[`array`](../../data-types.md) | Array of user IDs with access to the full report.

Filled if `REPORT_FULL_TYPE` is set to `user`  ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ACTIVE":true,"MINIMUM_IDLE_FOR_REPORT":15,"REGISTER_OFFLINE":true,"REGISTER_IDLE":true,"REGISTER_DESKTOP":true,"REPORT_REQUEST_TYPE":"all","REPORT_SIMPLE_TYPE":"all","REPORT_FULL_TYPE":"all"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.timecontrol.settings.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ACTIVE":true,"MINIMUM_IDLE_FOR_REPORT":15,"REGISTER_OFFLINE":true,"REGISTER_IDLE":true,"REGISTER_DESKTOP":true,"REPORT_REQUEST_TYPE":"all","REPORT_SIMPLE_TYPE":"all","REPORT_FULL_TYPE":"all","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.timecontrol.settings.set
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'timeman.timecontrol.settings.set',
        params: {
          ACTIVE: true,
          MINIMUM_IDLE_FOR_REPORT: 15,
          REGISTER_OFFLINE: true,
          REGISTER_IDLE: true,
          REGISTER_DESKTOP: true,
          REPORT_REQUEST_TYPE: 'all',
          REPORT_SIMPLE_TYPE: 'all',
          REPORT_FULL_TYPE: 'all',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Settings saved:', result)
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
      async function setTimeControlSettings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'timeman.timecontrol.settings.set',
            params: {
              ACTIVE: true,
              MINIMUM_IDLE_FOR_REPORT: 15,
              REGISTER_OFFLINE: true,
              REGISTER_IDLE: true,
              REGISTER_DESKTOP: true,
              REPORT_REQUEST_TYPE: 'all',
              REPORT_SIMPLE_TYPE: 'all',
              REPORT_FULL_TYPE: 'all',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Settings saved:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setTimeControlSettings)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.timecontrol.settings.set',
                [
                    'ACTIVE'                 => true,
                    'MINIMUM_IDLE_FOR_REPORT' => 15,
                    'REGISTER_OFFLINE'        => true,
                    'REGISTER_IDLE'           => true,
                    'REGISTER_DESKTOP'        => true,
                    'REPORT_REQUEST_TYPE'     => 'all',
                    'REPORT_SIMPLE_TYPE'      => 'all',
                    'REPORT_FULL_TYPE'        => 'all',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting time control settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'timeman.timecontrol.settings.set',
        {
            'ACTIVE': true,
            'MINIMUM_IDLE_FOR_REPORT': 15,
            'REGISTER_OFFLINE': true,
            'REGISTER_IDLE': true,
            'REGISTER_DESKTOP': true,
            'REPORT_REQUEST_TYPE': 'all',
            'REPORT_SIMPLE_TYPE': 'all',
            'REPORT_FULL_TYPE': 'all'
        },
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
        'timeman.timecontrol.settings.set',
        [
            'ACTIVE' => true,
            'MINIMUM_IDLE_FOR_REPORT' => 15,
            'REGISTER_OFFLINE' => true,
            'REGISTER_IDLE' => true,
            'REGISTER_DESKTOP' => true,
            'REPORT_REQUEST_TYPE' => 'all',
            'REPORT_SIMPLE_TYPE' => 'all',
            'REPORT_FULL_TYPE' => 'all'
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
    "result": true,
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
[`boolean`](../../data-types.md) | Execution result.

Returns `true` if the settings were successfully saved ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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

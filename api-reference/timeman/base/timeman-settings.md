# Get User Work Time Settings timeman.settings

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.settings` retrieves the user's work time settings.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID**
[`integer`](../../data-types.md) | User identifier.

By default — identifier of the current user ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":503}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.settings
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":503,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.settings
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TimemanSettingsResult = {
      UF_TIMEMAN: boolean
      UF_TM_FREE: boolean
      UF_TM_MAX_START: string
      UF_TM_MIN_FINISH: string
      UF_TM_MIN_DURATION: string
      UF_TM_ALLOWED_DELTA: string
      ADMIN: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<TimemanSettingsResult>({
        method: 'timeman.settings',
        params: {
          USER_ID: 503,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Time tracking enabled:', result.UF_TIMEMAN, 'Free schedule:', result.UF_TM_FREE, 'Admin:', result.ADMIN)
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
      async function fetchTimemanSettings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'timeman.settings',
            params: {
              USER_ID: 503,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Time tracking enabled:', result.UF_TIMEMAN, 'Free schedule:', result.UF_TM_FREE, 'Admin:', result.ADMIN)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchTimemanSettings)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.settings',
                [
                    'USER_ID' => 503
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching timeman settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'timeman.settings',
        {
            'USER_ID' : 503
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
        'timeman.settings',
        [
            'USER_ID' => 503
        ]
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
        "UF_TIMEMAN": true,
        "UF_TM_FREE": false,
        "UF_TM_MAX_START": "09:15:00",
        "UF_TM_MIN_FINISH": "17:45:00",
        "UF_TM_MIN_DURATION": "08:00:00",
        "UF_TM_ALLOWED_DELTA": "00:15:00",
        "ADMIN": true
    },
    "time": {
        "start": 1742994169.701416,
        "finish": 1742994169.7355511,
        "duration": 0.03413510322570801,
        "processing": 0.0076749324798583984,
        "date_start": "2025-03-26T16:02:49+02:00",
        "date_finish": "2025-03-26T16:02:49+02:00",
        "operating_reset_at": 1742994769,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response.

Contains an object with the user's work time settings ||
|| **UF_TIMEMAN**
[`boolean`](../../data-types.md) | Whether work time tracking is enabled for the user.

Returns `true` if enabled ||
|| **UF_TM_FREE**
[`boolean`](../../data-types.md) | Whether the user has a flexible work schedule.

Returns `true` if enabled.

A user with a flexible schedule does not need to confirm changes to work time with a supervisor or provide reasons for changes ||
|| **UF_TM_MAX_START**
[`string`](../../data-types.md) | Maximum start time of the workday in `HH:MM:SS` format.

Starting the workday later than the set time is considered a violation ||
|| **UF_TM_MIN_FINISH**
[`string`](../../data-types.md) | Minimum end time of the workday in `HH:MM:SS` format.

Ending the workday earlier than the set time is considered a violation ||
|| **UF_TM_MIN_DURATION**
[`string`](../../data-types.md) | Minimum duration of the workday in `HH:MM:SS` format.

A workday with a duration shorter than the set time is considered a violation ||
|| **UF_TM_ALLOWED_DELTA**
[`string`](../../data-types.md) | Allowed time change for work hours in `HH:MM:SS` format.

Changing the workday by a period shorter than the set time does not require supervisor confirmation ||
|| **ADMIN**
[`boolean`](../../data-types.md) | Whether the user can manage the workdays of other employees. Returned only for the current user.

Returns `true` if permissions are granted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"",
    "error_description":"User not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| empty string | User not found | User with the specified `USER_ID` not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-open.md)
- [{#T}](./timeman-pause.md)
- [{#T}](./timeman-close.md)
- [{#T}](./timeman-status.md)

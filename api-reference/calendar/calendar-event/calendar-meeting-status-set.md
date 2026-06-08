# Set Participation Status in Event for Current User calendar.meeting.status.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sets the participation status in an event for the current user.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **eventId***
[`integer`](../../data-types.md) | Event identifier.

You can obtain the identifier using the [calendar.event.get](./calendar-event-get.md) or [calendar.event.get.nearest](./calendar-event-get-nearest.md) methods ||
|| **status***
[`string`](../../data-types.md) | Participation status in the event. Possible values: 
- `Y` — accepted
- `N` — declined
- `Q` — invited, but not yet responded ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"eventId":651,"status":"Y"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.meeting.status.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"eventId":651,"status":"Y","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.meeting.status.set
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MeetingStatusSetResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<MeetingStatusSetResult>({
        method: 'calendar.meeting.status.set',
        params: {
          eventId: 651,
          status: 'Y',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Meeting status set successfully:', result)
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
      async function setMeetingStatus() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'calendar.meeting.status.set',
            params: {
              eventId: 651,
              status: 'Y',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Meeting status set successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setMeetingStatus)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'calendar.meeting.status.set',
                [
                    'eventId' => 651,
                    'status'  => 'Y',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting meeting status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.meeting.status.set',
        {
            eventId: 651,
            status: 'Y'
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.meeting.status.set',
        [
            'eventId' => 651,
            'status' => 'Y'
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
    "result": true,
    "time": {
        "start": 1728732695.718461,
        "finish": 1728732696.029712,
        "duration": 0.3112509250640869,
        "processing": 0.051657915115356445,
        "date_start": "2024-12-10T11:31:35+00:00",
        "date_finish": "2024-12-10T11:31:36+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Success of setting the status.

Returns `true` if the status is set successfully ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "status" for the method "calendar.meeting.status.set" is not set"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "status" for the method "calendar.meeting.status.set" is not set | The required parameter `status` is not provided ||
|| Empty string | The required parameter "eventId" for the method "calendar.meeting.status.set" is not set | The required parameter `eventId` is not provided ||
|| Empty string | Invalid value for the parameter "status" | The value provided for the `status` parameter is different from `Q`, `Y`, or `N` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-meeting-status-get.md)
- [{#T}](./calendar-accessibility-get.md)
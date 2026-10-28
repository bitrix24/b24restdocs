# Get User Availability from calendar.accessibility.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `calendar.accessibility.get` method retrieves the availability of users from the list.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **users***
[`array`](../../data-types.md) | Array of user IDs ||
|| **from***
[`date`](../../data-types.md) | Start date of the period for determining availability in the format `YYYY-MM-DD`.

For example, `2024-06-20` ||
|| **to***
[`date`](../../data-types.md) | End date of the period for determining availability in the format `YYYY-MM-DD`.

For example, `2024-12-20`  ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"from":"2024-06-20","to":"2024-12-20","users":[1,2,34]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.accessibility.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"from":"2024-06-20","to":"2024-12-20","users":[1,2,34],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.accessibility.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CalendarEventItem = {
      ID: string
      NAME: string
      DATE_FROM: string
      DATE_TO: string
      DATE_FROM_TS_UTC: string
      DATE_TO_TS_UTC: string
      '~USER_OFFSET_FROM': number
      '~USER_OFFSET_TO': number
      DT_SKIP_TIME: 'Y' | 'N'
      TZ_FROM: string
      TZ_TO: string
      ACCESSIBILITY: 'busy' | 'absent' | 'quest' | 'free'
      IMPORTANCE: 'high' | 'normal' | 'low'
      EVENT_TYPE: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AccessibilityGetResult = Record<string, CalendarEventItem[]>

    try {
      const response = await $b24.actions.v2.call.make<AccessibilityGetResult>({
        method: 'calendar.accessibility.get',
        params: {
          from: '2024-06-20',
          to: '2024-12-20',
          users: [1, 2, 34],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        for (const [userId, events] of Object.entries(result)) {
          console.info(`User ${userId}:`, events.map(e => `${e.NAME} (${e.ACCESSIBILITY})`))
        }
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
      async function getAccessibility() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'calendar.accessibility.get',
            params: {
              from: '2024-06-20',
              to: '2024-12-20',
              users: [1, 2, 34],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          for (const [userId, events] of Object.entries(result)) {
            console.info(`User ${userId}:`, events.map(e => `${e.NAME} (${e.ACCESSIBILITY})`))
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAccessibility)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.calendar.accessibility.get(
            users=[
                1,
                2,
                34,
            ],
            from_date="2024-06-20",
            to="2024-12-20",
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'calendar.accessibility.get',
                [
                    'from'  => '2024-06-20',
                    'to'    => '2024-12-20',
                    'users' => [1, 2, 34]
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
        echo 'Error getting calendar accessibility: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.accessibility.get',
        {
            from: '2024-06-20',
            to: '2024-12-20',
            users: [1, 2, 34]
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.accessibility.get',
        [
            'from' => '2024-06-20',
            'to' => '2024-12-20',
            'users' => [1, 2, 34]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "calendar.accessibility.get", b24.Params{
    	"from":  "2024-06-20",
    	"to":    "2024-12-20",
    	"users": []int{1, 2, 34},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("calendar.accessibility.get: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "1269": [
            {
                "ID": "8373",
                "NAME": "Sales team stand-up",
                "DATE_FROM": "11/20/2026 10:00:00 am",
                "DATE_TO": "11/20/2026 11:00:00 am",
                "DATE_FROM_TS_UTC": "1795147200",
                "DATE_TO_TS_UTC": "1795150800",
                "~USER_OFFSET_FROM": 0,
                "~USER_OFFSET_TO": 0,
                "DT_SKIP_TIME": "N",
                "TZ_FROM": "Europe/Berlin",
                "TZ_TO": "Europe/Berlin",
                "ACCESSIBILITY": "busy",
                "IMPORTANCE": "normal",
                "EVENT_TYPE": null
            }
        ],
        "1271": [
            {
                "ID": "8375",
                "NAME": "Sales team stand-up",
                "DATE_FROM": "11/20/2026 10:00:00 am",
                "DATE_TO": "11/20/2026 11:00:00 am",
                "DATE_FROM_TS_UTC": "1795147200",
                "DATE_TO_TS_UTC": "1795150800",
                "~USER_OFFSET_FROM": 0,
                "~USER_OFFSET_TO": 0,
                "DT_SKIP_TIME": "N",
                "TZ_FROM": "Europe/Berlin",
                "TZ_TO": "Europe/Berlin",
                "ACCESSIBILITY": "busy",
                "IMPORTANCE": "normal",
                "EVENT_TYPE": null
            }
        ],
        "1": []
    },
    "time": {
        "start": 1793117690,
        "finish": 1793117690.055119,
        "duration": 0.05511903762817383,
        "processing": 0,
        "date_start": "2026-10-27T19:14:50+03:00",
        "date_finish": "2026-10-27T19:14:50+03:00",
        "operating_reset_at": 1793118240,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The result contains an object.

The key of the object is the user ID from the request.

The value is an array of objects, each describing an [event](#event) in which the user is busy during the specified period. If the user has no events in the period or the user is not found, the array is empty ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Event Object {#event}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Event ID ||
|| **NAME**
[`string`](../../data-types.md) | Event name ||
|| **DATE_FROM**
[`datetime`](../../data-types.md) | Start date and time of the event ||
|| **DATE_TO**
[`datetime`](../../data-types.md) | End date and time of the event ||
|| **DATE_FROM_TS_UTC**
[`string`](../../data-types.md) | Start date and time of the event in timestamp format.

Despite the field name, the value is shifted by the server time zone offset, so it cannot be read as UTC time. The exact start time is defined by the `DATE_FROM` and `TZ_FROM` fields ||
|| **DATE_TO_TS_UTC**
[`string`](../../data-types.md) | End date and time of the event in timestamp format.

Despite the field name, the value is shifted by the server time zone offset, so it cannot be read as UTC time. The exact end time is defined by the `DATE_TO` and `TZ_TO` fields ||
|| **~USER_OFFSET_FROM**
[`integer`](../../data-types.md) | Time offset of the start of the event relative to the current user's time zone in seconds ||
|| **~USER_OFFSET_TO**
[`integer`](../../data-types.md) | Time offset of the end of the event relative to the current user's time zone in seconds ||
|| **DT_SKIP_TIME**
[`string`](../../data-types.md) | Flag indicating that the event lasts all day. Possible values:
- `Y` — all day
- `N` — not all day ||
|| **TZ_FROM**
[`string`](../../data-types.md) | Time zone of the event start date ||
|| **TZ_TO**
[`string`](../../data-types.md) | Time zone of the event end date ||
|| **ACCESSIBILITY**
[`string`](../../data-types.md) | Availability of event participants. Possible values:

- `busy` — occupied
- `absent` — away
- `quest` — undecided
- `free` — free ||
|| **IMPORTANCE**
[`string`](../../data-types.md) | Importance of the event. Possible values:

- `high` — high
- `normal` — medium
- `low` — low ||
|| **EVENT_TYPE**
[`string`](../../data-types.md) | Some events contain information about how they were created.

An event can be created through:

- `#shared#` — calendar slots
- `#shared_crm#` — CRM slots
- `#collab#` — collaboration
- `#shared_collab#` — collaboration slots

For a regular event, `null` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The mandatory parameter \"from\" is not specified for the method \"calendar.accessibility.get\""
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | The mandatory parameter "from" is not specified for the method "calendar.accessibility.get" | The required parameter `from` is missing ||
|| `400` | Empty value | The mandatory parameter "to" is not specified for the method "calendar.accessibility.get" | The required parameter `to` is missing ||
|| `400` | Empty value | The mandatory parameter "users" is not specified for the method "calendar.accessibility.get" | The required parameter `users` is missing ||
|| `400` | Empty value | Access denied | Access to the method is prohibited for external users ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./calendar-meeting-status-get.md)
- [{#T}](./calendar-meeting-status-set.md)
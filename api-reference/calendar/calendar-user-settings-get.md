# Get User Calendar Settings calendar.user.settings.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `calendar.user.settings.get` method retrieves user calendar settings.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.user.settings.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.user.settings.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CalendarUserSettingsResult = {
      view: string
      meetSection: string | boolean
      crmSection: string | boolean
      showDeclined: boolean
      denyBusyInvitation: boolean
      collapseOffHours: string
      showWeekNumbers: string
      showTasks: string
      syncTasks: string
      showCompletedTasks: string
      lastUsedSection: string | boolean
      sendFromEmail: string
      defaultSections: Record<string, string>
      syncPeriodPast: string | number
      syncPeriodFuture: string | number
      defaultReminders: {
        fullDay:
          | { type: string; count?: number; before?: number; time?: number }
          | Array<{ type: string; count?: number; before?: number; time?: number }>
        withTime:
          | { type: string; count?: number; before?: number; time?: number }
          | Array<{ type: string; count?: number; before?: number; time?: number }>
      }
      tabId?: string
      view_crm?: string
      view_tasks?: string
      timezoneName: string
      timezoneOffsetUTC: number
      timezoneDefaultName: string
      work_time_start?: string
      work_time_end?: string
      CalendarSelCont: string | boolean
      SPCalendarSelCont: string | boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<CalendarUserSettingsResult>({
        method: 'calendar.user.settings.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.view, result.timezoneName, result.defaultReminders)
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
      async function getCalendarUserSettings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'calendar.user.settings.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.view, result.timezoneName, result.defaultReminders)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCalendarUserSettings)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.calendar.user.settings.get().response
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
                'calendar.user.settings.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Next, process $result in your code

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting user calendar settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.user.settings.get',
        {}
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.user.settings.get',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "calendar.user.settings.get", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("calendar.user.settings.get: %w", err)
    }

    // The structure is shortened: only the fields needed for the example are listed below.
    // The full list of fields is in the "Returned Data" section.
    var item struct {
    	View               string `json:"view"`
    	MeetSection        any    `json:"meetSection"`
    	CrmSection         any    `json:"crmSection"`
    	ShowDeclined       bool   `json:"showDeclined"`
    	DenyBusyInvitation bool   `json:"denyBusyInvitation"`
    	CollapseOffHours   string `json:"collapseOffHours"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.View, item.MeetSection)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "view": "month",
        "meetSection": "4",
        "crmSection": "4",
        "showDeclined": true,
        "denyBusyInvitation": false,
        "CalendarSelCont": "false",
        "SPCalendarSelCont": "false",
        "collapseOffHours": "N",
        "showWeekNumbers": "N",
        "showTasks": "Y",
        "syncTasks": "N",
        "showCompletedTasks": "N",
        "lastUsedSection": "false",
        "sendFromEmail": "",
        "defaultSections": {
            "user1": "4",
            "group6": "49"
        },
        "syncPeriodPast": "3",
        "syncPeriodFuture": "12",
        "defaultReminders": {
            "fullDay": [
                {
                    "type": "min",
                    "count": "15"
                }
            ],
            "withTime": [
                {
                    "type": "min",
                    "count": 50
                }
            ]
        },
        "tabId": "month",
        "view_crm": "month",
        "view_tasks": "month",
        "timezoneName": "Europe/Berlin",
        "timezoneOffsetUTC": 7200,
        "timezoneDefaultName": "",
        "work_time_start": "9.00",
        "work_time_end": "19.00"
    },
    "time": {
        "start": 1734434829.455204,
        "finish": 1734434829.797482,
        "duration": 0.34227800369262695,
        "processing": 0.0038161277770996094,
        "date_start": "2024-12-17T11:27:09+00:00",
        "date_finish": "2024-12-17T11:27:09+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response [(detailed description)](#settings) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Settings Object {#settings}

#|
|| **Name**
`type` | **Description** ||
|| **view**
[`string`](../data-types.md) | Standard view for the calendar. Possible values:
- `day` — day
- `week` — week
- `month` — month
- `list` — list ||
|| **meetSection**
[```string|boolean```](../data-types.md) | Identifier of the calendar for invitations. Until the setting is saved, the value comes as `false` ||
|| **crmSection**
[```string|boolean```](../data-types.md) | Identifier of the calendar for CRM. Until the setting is saved, the value comes as `false` ||
|| **showDeclined**
[`boolean`](../data-types.md) | Show events where the user declined to participate ||
|| **denyBusyInvitation**
[`boolean`](../data-types.md) | Prevent inviting to an event if the time is busy ||
|| **collapseOffHours**
[`string`](../data-types.md) | Hide non-working hours in the calendar in weekly and daily views. Possible values:
- `Y` — hide
- `N` — do not hide ||
|| **showWeekNumbers**
[`string`](../data-types.md) | Show week numbers. Possible values:
- `Y` — show
- `N` — do not show ||
|| **showTasks**
[`string`](../data-types.md) | Display tasks in the calendar. Possible values:
- `Y` — display
- `N` — do not display ||
|| **syncTasks**
[`string`](../data-types.md) | Synchronize task calendar. Possible values:
- `Y` — yes
- `N` — no ||
|| **showCompletedTasks**
[`string`](../data-types.md) | Display completed tasks. Possible values:
- `Y` — display
- `N` — do not display  ||
|| **lastUsedSection**
[```string|boolean```](../data-types.md) | Identifier of the calendar used when creating events if the calendar identifier is not provided in the parameters.

Until the setting is saved, the value comes as `false` — no calendar is selected ||
|| **sendFromEmail**
[`string`](../data-types.md) | E-mail for sending mail invitations ||
|| **defaultSections**
[`object`](../data-types.md) | Settings for preset calendars.

The key of the settings object can be:
- `user[id]` — type User calendar with user identifier. For example, `user12` corresponds to the user calendar with identifier `12`
- `group[id]` — type Group calendar with group identifier. For example, `group36` corresponds to the group calendar with identifier `36`

The value is the calendar identifier ||
|| **syncPeriodPast**
[```string|integer```](../data-types.md) | Number of months for synchronization in the past period. Until the setting is saved, the value comes as the number `3` ||
|| **syncPeriodFuture**
[```string|integer```](../data-types.md) | Number of months for synchronization in the future period. Until the setting is saved, the value comes as the number `12` ||
|| **defaultReminders**
[`object`](../data-types.md) | Object with standard settings for event reminders [(detailed description)](#defaultReminders) ||
|| **tabId**
[`string`](../data-types.md) | Last opened calendar tab. The field comes only if the calendar interface saved it. The [calendar.user.settings.set](./calendar-user-settings-set.md) method does not accept this field ||
|| **view_crm**
[`string`](../data-types.md) | Default view for the CRM calendar. Possible values:
- `day` — day
- `week` — week
- `month` — month
- `list` — list

The field comes only if the calendar interface saved it. The [calendar.user.settings.set](./calendar-user-settings-set.md) method does not accept this field ||
|| **view_tasks**
[`string`](../data-types.md) | Default view for the task calendar. Possible values:
- `day` — day
- `week` — week
- `month` — month
- `list` — list

The field comes only if the calendar interface saved it. The [calendar.user.settings.set](./calendar-user-settings-set.md) method does not accept this field ||
|| **timezoneName**
[`string`](../data-types.md) | Calendar timezone ||
|| **timezoneOffsetUTC**
[`integer`](../data-types.md) | Timezone offset relative to UTC in seconds ||
|| **timezoneDefaultName**
[`string`](../data-types.md) | Timezone that Bitrix24 offers to the user if `timezoneName` is not set. The value is selected by the `timezoneOffsetUTC` offset ||
|| **work_time_start**
[`string`](../data-types.md) | Start time of the workday. The field comes only if the user has working hours filled in the calendar ||
|| **work_time_end**
[`string`](../data-types.md) | End time of the workday. The field comes only if the user has working hours filled in the calendar ||
|| **CalendarSelCont**
[```string|boolean```](../data-types.md) | Service field of the calendar interface. Until the setting is saved, the value comes as `false`, after saving — as the string `"true"` or `"false"` ||
|| **SPCalendarSelCont**
[```string|boolean```](../data-types.md) | Service field of the calendar interface. Until the setting is saved, the value comes as `false`, after saving — as the string `"true"` or `"false"` ||
|#

#### Object defaultReminders {#defaultReminders}

#|
|| **Name**
`type` | **Description** ||
|| **fullDay**
[```object|array```](../data-types.md) | Reminder settings for all-day events: a single object or an array of objects [(detailed description)](#reminder-settings). Until the setting is saved, the value comes as an object: `{"type": "daybefore", "before": 0, "time": 480}` ||
|| **withTime**
[```object|array```](../data-types.md) | Reminder settings for events with specified time: a single object or an array of objects [(detailed description)](#reminder-settings). Until the setting is saved, the value comes as an object: `{"type": "min", "count": 15}` ||
|#

If the reminder settings were passed to [calendar.user.settings.set](./calendar-user-settings-set.md), the method returns them in the same form: an object or an array of objects.

#### Reminder Settings Object {#reminder-settings}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../data-types.md) | Time type of the reminder. Possible values:
- `min` — minutes
- `hour` — hours
- `day` — days
- `daybefore` — reminder at the specified time of the event day ||
|| **count**
[```integer|string```](../data-types.md) | Numeric value of the time interval. Used with the `min`, `hour`, and `day` types. Can come as a number or a string ||
|| **before**
[`integer`](../data-types.md) | Reminder shift in days before the event day. Used with the `daybefore` type. With `0`, the reminder comes on the event day ||
|| **time**
[`integer`](../data-types.md) | Reminder time in minutes from the start of the day. Used with the `daybefore` type. With `before: 0` and `time: 480`, the reminder comes on the event day at 08:00 ||
|#

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-settings-get.md)
- [{#T}](./calendar-user-settings-set.md)
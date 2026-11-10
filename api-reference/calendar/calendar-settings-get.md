# Get main calendar settings calendar.settings.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `calendar.settings.get` method retrieves the main calendar settings: the company's working hours, weekends and holidays, link templates, and meeting room parameters.

Only an administrator of the Bitrix24 account can modify the main settings, in the interface. An employee's user settings are retrieved and modified by the [calendar.user.settings.get](./calendar-user-settings-get.md) and [calendar.user.settings.set](./calendar-user-settings-set.md) methods.

For extranet users, the method returns only `work_time_start` and `work_time_end`, and the other fields are absent from the response. The values of these fields for extranet users are fixed — `9` and `19` — rather than the company's working hours.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote about examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.settings.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.settings.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CalendarSettingsResult = {
      work_time_start: string | number
      work_time_end: string | number
      year_holidays: string
      year_workdays: string
      week_holidays: string[]
      week_start: string
      user_name_template: string
      sync_by_push: string | boolean
      user_show_login: string | boolean
      path_to_user: string
      path_to_user_calendar: string
      path_to_group: string
      path_to_group_calendar: string
      path_to_vr: string
      path_to_rm: string
      rm_iblock_type: string
      rm_iblock_id: string
      dep_manager_sub: string | boolean
      denied_superpose_types: string[]
      pathes_for_sites: string | boolean
      pathes: Array<Record<string, string>>
      forum_id: string
      rm_for_sites: string | boolean
      path_to_type_company_calendar: string
      path_to_type_location: string
      path_to_type_events: string
      path_to_type_resource: string
    }

    try {
      const response = await $b24.actions.v2.call.make<CalendarSettingsResult>({
        method: 'calendar.settings.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Calendar settings:', result.work_time_start, result.work_time_end)
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
      async function getCalendarSettings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'calendar.settings.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Calendar settings:', result.work_time_start, result.work_time_end)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCalendarSettings)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.calendar.settings.get().response
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
                'calendar.settings.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Next, process $result in your code

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting calendar settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.settings.get',
        {}
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.settings.get',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "calendar.settings.get", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("calendar.settings.get: %w", err)
    }

    // The structure is shortened: only the fields needed for the example are listed below.
    // The full list of fields is in the "Returned Data" section.
    var item struct {
    	WorkTimeStart    any    `json:"work_time_start"`
    	WorkTimeEnd      any    `json:"work_time_end"`
    	YearHolidays     string `json:"year_holidays"`
    	YearWorkdays     string `json:"year_workdays"`
    	WeekStart        string `json:"week_start"`
    	UserNameTemplate string `json:"user_name_template"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.WorkTimeStart, item.WorkTimeEnd)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "work_time_start": "9",
        "work_time_end": "19",
        "year_holidays": "01.01,04.07,01.11,25.12",
        "year_workdays": "31.12",
        "week_holidays": [
            "SA",
            "SU"
        ],
        "week_start": "MO",
        "user_name_template": "#NAME# #LAST_NAME#",
        "sync_by_push": "",
        "user_show_login": "1",
        "path_to_user": "/company/personal/user/#user_id#/",
        "path_to_user_calendar": "/company/personal/user/#user_id#/calendar/",
        "path_to_group": "/workgroups/group/#group_id#/",
        "path_to_group_calendar": "/workgroups/group/#group_id#/calendar/",
        "path_to_vr": "",
        "path_to_rm": "",
        "rm_iblock_type": "",
        "rm_iblock_id": "",
        "dep_manager_sub": "1",
        "denied_superpose_types": [],
        "pathes_for_sites": "",
        "pathes": [],
        "forum_id": "8",
        "rm_for_sites": "1",
        "path_to_type_company_calendar": "",
        "path_to_type_location": "",
        "path_to_type_events": "",
        "path_to_type_resource": ""
    },
    "time": {
        "start": 1733924639.802569,
        "finish": 1733924640.184363,
        "duration": 0.3817939758300781,
        "processing": 0.012382984161376953,
        "date_start": "2024-12-11T13:43:59+00:00",
        "date_finish": "2024-12-11T13:44:00+00:00"
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
|| **work_time_start**
[```string|integer```](../data-types.md) | Start time of the workday. Until the setting is saved, the value comes as the number `9`; after it is saved, usually as a string. If the saved value is greater than `23`, the method returns the number `23` ||
|| **work_time_end**
[```string|integer```](../data-types.md) | End time of the workday. Until the setting is saved, the value comes as the number `19`; after it is saved, usually as a string. If the end time is not later than the start time, the method returns a number — the start time plus one hour. If the value is greater than `23.30`, the method returns the number `23.3` ||
|| **year_holidays**
[`string`](../data-types.md) | Holidays in the year: dates separated by commas in `day.month` format, for example `01.01,04.07,01.11,25.12` ||
|| **year_workdays**
[`string`](../data-types.md) | Working days in the year: dates separated by commas in `day.month` format, for example `31.12` ||
|| **week_holidays**
[`array`](../data-types.md) | Array of weekend days. An array element is a [`string`](../data-types.md): a two-letter day-of-week code, `MO`, `TU`, `WE`, `TH`, `FR`, `SA`, `SU` ||
|| **week_start**
[`string`](../data-types.md) | Day the week starts ||
|| **user_name_template**
[`string`](../data-types.md) | User name template ||
|| **sync_by_push**
[```string|boolean```](../data-types.md) | Flag for automatic calendar synchronization via subscription to Google and Office365 push events. Until the setting is saved, the value comes as `false`; after it is saved, as a string: `'1'` — enabled, an empty string — disabled ||
|| **user_show_login**
[```string|boolean```](../data-types.md) | Flag for displaying user login. Until the setting is saved, the value comes as `true`; after it is saved, as a string: `'1'` — enabled, an empty string — disabled ||
|| **path_to_user**
[`string`](../data-types.md) | Template link to user profile ||
|| **path_to_user_calendar**
[`string`](../data-types.md) | Template link to view user calendar ||
|| **path_to_group**
[`string`](../data-types.md) | Template link to view workgroup ||
|| **path_to_group_calendar**
[`string`](../data-types.md) | Template link to view group calendar ||
|| **path_to_vr**
[`string`](../data-types.md) | Template link to video conference room ||
|| **path_to_rm**
[`string`](../data-types.md) | Template link to meeting room ||
|| **rm_iblock_type**
[`string`](../data-types.md) | Type of infoblock for booking meeting and video conference rooms ||
|| **rm_iblock_id**
[`string`](../data-types.md) | Identifier of the infoblock for booking meeting rooms ||
|| **dep_manager_sub**
[```string|boolean```](../data-types.md) | Flag allowing managers to view subordinates' calendars. Until the setting is saved, the value comes as `true`; after it is saved, as a string: `'1'` — enabled, an empty string — disabled ||
|| **denied_superpose_types**
[`array`](../data-types.md) | List of calendar types that cannot be added to favorites. An array element is a [`string`](../data-types.md), for example `user` or `group` ||
|| **pathes_for_sites**
[```string|boolean```](../data-types.md) | Flag that enables link templates common for all sites. Until the setting is saved, the value comes as `true`; after it is saved, as a string. An empty string means the templates are set separately for each site ||
|| **pathes**
[`array`](../data-types.md) | Array of link templates. The method does not pass the site code, so link templates set separately for sites do not appear in the response. If the array is not empty, its element is an object where the key is the path name, for example `path_to_user`, and the value is the link template ||
|| **forum_id**
[`string`](../data-types.md) | Identifier of the forum for comments ||
|| **rm_for_sites**
[```string|boolean```](../data-types.md) | Flag for meeting room parameters common to all sites. Until the setting is saved, the value comes as `true`; after it is saved, as a string: `'1'` — enabled, an empty string — disabled ||
|| **path_to_type_company_calendar**
[`string`](../data-types.md) | Template link to view company calendars ||
|| **path_to_type_location**
[`string`](../data-types.md) | Template link to view meeting room bookings ||
|| **path_to_type_events**
[`string`](../data-types.md) | Template link to view the event calendar ||
|| **path_to_type_open_event**
[`string`](../data-types.md) | Template link to view open event calendar ||
|| **path_to_type_resource**
[`string`](../data-types.md) | Template link to view the resource calendar ||
|#

The `path_to_type_*` keys are built from the calendar types registered in the Bitrix24 account: for each type except `user` and `group`, the method returns a `path_to_type_<XML_ID>` key. For example, for the type with the code `open_event`, the `path_to_type_open_event` key is returned. The set of keys differs from one Bitrix24 to another.

## Error Handling

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-user-settings-get.md)
- [{#T}](./calendar-user-settings-set.md)
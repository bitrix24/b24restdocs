# Set User Calendar Settings calendar.user.settings.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method `calendar.user.settings.set` sets user calendar settings for the current user.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **settings***
[`object`](../data-types.md) | An object with values for user calendar settings [(detailed description)](#settings) ||
|#

### Parameter settings {#settings}

All fields of the `settings` object are optional. Top-level fields that are not passed are left unchanged by the method. If `defaultSections` or `defaultReminders` is passed, the saved value is replaced entirely: nested keys that are missing in the new value are removed.

The `timezoneName`, `timezoneOffsetUTC`, `timezoneDefaultName`, `work_time_start`, and `work_time_end` fields are not set by this method — they are returned by [calendar.user.settings.get](./calendar-user-settings-get.md) and changed by the employee in the calendar interface.

The method saves values without validation: it ignores unknown keys and writes an invalid value of a known key as is. For example, `view: "zzz"` is saved and returned by `calendar.user.settings.get`. Validate the values on your side.

#|
|| **Name**
`type` | **Description** ||
|| **view**
[`string`](../data-types.md) | Default view for the calendar. Possible values:
- `day` — day
- `week` — week
- `month` — month
- `list` — list ||
|| **meetSection**
[```string|boolean```](../data-types.md) | Identifier of the calendar for invitations. Default value — `false` ||
|| **crmSection**
[```string|boolean```](../data-types.md) | Identifier of the calendar for CRM. Default value — `false` ||
|| **showDeclined**
[`boolean`](../data-types.md) | Show events that the user declined to participate in ||
|| **denyBusyInvitation**
[`boolean`](../data-types.md) | Prevent inviting to an event if the time is busy ||
|| **CalendarSelCont**
[```string|boolean```](../data-types.md) | Service field of the calendar interface. Default value — `false` ||
|| **SPCalendarSelCont**
[```string|boolean```](../data-types.md) | Service field of the calendar interface. Default value — `false` ||
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
- `N` — do not display ||
|| **lastUsedSection**
[```string|boolean```](../data-types.md) | Identifier of the calendar used when creating events if the calendar identifier is not provided in the parameters.

Default value — `false`, no calendar is selected ||
|| **sendFromEmail**
[`string`](../data-types.md) | E-mail for sending mail invitations ||
|| **defaultSections**
[`object`](../data-types.md) | Settings for preset calendars.

The key of the settings object can be:
- `user[id]` — User Calendar type with user identifier. For example, `user12` corresponds to the calendar of the user with identifier `12`
- `group[id]` — Group Calendar type with group identifier. For example, `group36` corresponds to the calendar of the group with identifier `36`

The value is the calendar identifier ||
|| **syncPeriodPast**
[```string|integer```](../data-types.md) | Number of months for synchronization in the past period. Default value — `3` ||
|| **syncPeriodFuture**
[```string|integer```](../data-types.md) | Number of months for synchronization in the future period. Default value — `12` ||
|| **defaultReminders**
[`object`](../data-types.md) | An object with standard event reminder settings [(detailed description)](#defaultReminders) ||
|#

### Object defaultReminders {#defaultReminders}

#|
|| **Name**
`type` | **Description** ||
|| **fullDay**
[```object|array```](../data-types.md) | Reminder settings for all-day events: a single object or an array of objects [(detailed description)](#reminder-settings) ||
|| **withTime**
[```object|array```](../data-types.md) | Reminder settings for timed events: a single object or an array of objects [(detailed description)](#reminder-settings) ||
|#

The `defaultReminders` value is replaced entirely: to change a single reminder, pass both object fields in the required form.

### Reminder settings object {#reminder-settings}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../data-types.md) | Time type of reminder. Possible values:
- `min` — minutes
- `hour` — hours
- `day` — days
- `daybefore` — reminder at the specified time of the event day ||
|| **count**
[```integer|string```](../data-types.md) | Numeric value of the time interval. Used with the `min`, `hour`, and `day` types. Can be passed as a number or a string ||
|| **before**
[`integer`](../data-types.md) | Reminder shift in days before the event day. Used with the `daybefore` type. With `0`, the reminder comes on the event day ||
|| **time**
[`integer`](../data-types.md) | Reminder time in minutes from the start of the day. Used with the `daybefore` type. With `before: 0` and `time: 480`, the reminder comes on the event day at 08:00 ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"settings":{"view":"month","meetSection":"4","crmSection":"4","showDeclined":true,"denyBusyInvitation":false,"collapseOffHours":"N","showWeekNumbers":"N","showTasks":"Y","syncTasks":"N","showCompletedTasks":"N","lastUsedSection":"false","sendFromEmail":"","defaultSections":{"user1":"4","group6":"49"},"syncPeriodPast":"3","syncPeriodFuture":"12","defaultReminders":{"fullDay":[{"type":"min","count":15}],"withTime":[{"type":"min","count":50}]}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.user.settings.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"settings":{"view":"month","meetSection":"4","crmSection":"4","showDeclined":true,"denyBusyInvitation":false,"collapseOffHours":"N","showWeekNumbers":"N","showTasks":"Y","syncTasks":"N","showCompletedTasks":"N","lastUsedSection":"false","sendFromEmail":"","defaultSections":{"user1":"4","group6":"49"},"syncPeriodPast":"3","syncPeriodFuture":"12","defaultReminders":{"fullDay":[{"type":"min","count":15}],"withTime":[{"type":"min","count":50}]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.user.settings.set
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CalendarUserSettingsSetResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<CalendarUserSettingsSetResult>({
        method: 'calendar.user.settings.set',
        params: {
          settings: {
            view: 'month',
            meetSection: '4',
            crmSection: '4',
            showDeclined: true,
            denyBusyInvitation: false,
            collapseOffHours: 'N',
            showWeekNumbers: 'N',
            showTasks: 'Y',
            syncTasks: 'N',
            showCompletedTasks: 'N',
            lastUsedSection: 'false',
            sendFromEmail: '',
            defaultSections: {
              user1: '4',
              group6: '49',
            },
            syncPeriodPast: '3',
            syncPeriodFuture: '12',
            defaultReminders: {
              fullDay: [
                {
                  type: 'min',
                  count: 15,
                },
              ],
              withTime: [
                {
                  type: 'min',
                  count: 50,
                },
              ],
            },
          },
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
      async function setCalendarUserSettings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'calendar.user.settings.set',
            params: {
              settings: {
                view: 'month',
                meetSection: '4',
                crmSection: '4',
                showDeclined: true,
                denyBusyInvitation: false,
                collapseOffHours: 'N',
                showWeekNumbers: 'N',
                showTasks: 'Y',
                syncTasks: 'N',
                showCompletedTasks: 'N',
                lastUsedSection: 'false',
                sendFromEmail: '',
                defaultSections: {
                  user1: '4',
                  group6: '49',
                },
                syncPeriodPast: '3',
                syncPeriodFuture: '12',
                defaultReminders: {
                  fullDay: [
                    {
                      type: 'min',
                      count: 15,
                    },
                  ],
                  withTime: [
                    {
                      type: 'min',
                      count: 50,
                    },
                  ],
                },
              },
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

      document.addEventListener('DOMContentLoaded', setCalendarUserSettings)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.calendar.user.settings.set(
            settings={
                "view": "month",
                "meetSection": "4",
                "crmSection": "4",
                "showDeclined": True,
                "denyBusyInvitation": False,
                "collapseOffHours": "N",
                "showWeekNumbers": "N",
                "showTasks": "Y",
                "syncTasks": "N",
                "showCompletedTasks": "N",
                "lastUsedSection": "false",
                "sendFromEmail": "",
                "defaultSections": {
                    "user1": "4",
                    "group6": "49",
                },
                "syncPeriodPast": "3",
                "syncPeriodFuture": "12",
                "defaultReminders": {
                    "fullDay": [
                        {
                            "type": "min",
                            "count": 15,
                        },
                    ],
                    "withTime": [
                        {
                            "type": "min",
                            "count": 50,
                        },
                    ],
                },
            },
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
                'calendar.user.settings.set',
                [
                    'settings' => [
                        'view'              => 'month',
                        'meetSection'       => '4',
                        'crmSection'        => '4',
                        'showDeclined'      => true,
                        'denyBusyInvitation' => false,
                        'collapseOffHours'  => 'N',
                        'showWeekNumbers'   => 'N',
                        'showTasks'         => 'Y',
                        'syncTasks'         => 'N',
                        'showCompletedTasks' => 'N',
                        'lastUsedSection'   => 'false',
                        'sendFromEmail'     => '',
                        'defaultSections'   => [
                            'user1'  => '4',
                            'group6' => '49'
                        ],
                        'syncPeriodPast'    => '3',
                        'syncPeriodFuture'  => '12',
                        'defaultReminders'  => [
                            'fullDay'  => [
                                [
                                    'type'  => 'min',
                                    'count' => 15
                                ]
                            ],
                            'withTime' => [
                                [
                                    'type'  => 'min',
                                    'count' => 50
                                ]
                            ]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Next, process $result in your code

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting user calendar settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.user.settings.set',
        {
            settings: {
                view: 'month',
                meetSection: '4',
                crmSection: '4',
                showDeclined: true,
                denyBusyInvitation: false,
                collapseOffHours: 'N',
                showWeekNumbers: 'N',
                showTasks: 'Y',
                syncTasks: 'N',
                showCompletedTasks: 'N',
                lastUsedSection: 'false',
                sendFromEmail: '',
                defaultSections: {
                    user1: '4',
                    group6: '49'
                },
                syncPeriodPast: '3',
                syncPeriodFuture: '12',
                defaultReminders: {
                    fullDay: [
                        {
                            type: 'min',
                            count: 15
                        }
                    ],
                    withTime: [
                        {
                            type: 'min',
                            count: 50
                        }
                    ]
                }
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.user.settings.set',
        [
            'settings' => [
                'view' => 'month',
                'meetSection' => '4',
                'crmSection' => '4',
                'showDeclined' => true,
                'denyBusyInvitation' => false,
                'collapseOffHours' => 'N',
                'showWeekNumbers' => 'N',
                'showTasks' => 'Y',
                'syncTasks' => 'N',
                'showCompletedTasks' => 'N',
                'lastUsedSection' => 'false',
                'sendFromEmail' => '',
                'defaultSections' => [
                    'user1' => '4',
                    'group6' => '49'
                ],
                'syncPeriodPast' => '3',
                'syncPeriodFuture' => '12',
                'defaultReminders' => [
                    'fullDay' => [
                        [
                            'type' => 'min',
                            'count' => 15
                        ]
                    ],
                    'withTime' => [
                        [
                            'type' => 'min',
                            'count' => 50
                        ]
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "calendar.user.settings.set", b24.Params{
    	"settings": b24.Params{
    		"view":               "month",
    		"meetSection":        "4",
    		"crmSection":         "4",
    		"showDeclined":       true,
    		"denyBusyInvitation": false,
    		"collapseOffHours":   "N",
    		"showWeekNumbers":    "N",
    		"showTasks":          "Y",
    		"syncTasks":          "N",
    		"showCompletedTasks": "N",
    		"lastUsedSection":    "false",
    		"sendFromEmail":      "",
    		"defaultSections": b24.Params{
    			"user1":  "4",
    			"group6": "49",
    		},
    		"syncPeriodPast":   "3",
    		"syncPeriodFuture": "12",
    		"defaultReminders": b24.Params{
    			"fullDay": []b24.Params{
    				{
    					"type":  "min",
    					"count": 15,
    				},
    			},
    			"withTime": []b24.Params{
    				{
    					"type":  "min",
    					"count": 50,
    				},
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("calendar.user.settings.set: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1733318565.183275,
        "finish": 1733318565.695058,
        "duration": 0.5117831230163574,
        "processing": 0.29406094551086426,
        "date_start": "2024-12-04T13:22:45+00:00",
        "date_finish": "2024-12-04T13:22:45+00:00"
    }
}
```
### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Returns `true` if the execution was successful ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The mandatory parameter \"settings\" is not specified for the method \"calendar.user.settings.set\""
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | The mandatory parameter "settings" is not specified for the method "calendar.user.settings.set" | The required parameter `settings` was not provided ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-settings-get.md)
- [{#T}](./calendar-user-settings-get.md)

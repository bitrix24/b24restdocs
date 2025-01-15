# Set User Calendar Settings calendar.user.settings.set

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method sets user calendar settings.

## Method Parameters

{% include [Footnote on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **settings***
[`object`](../data-types.md) | An object containing user [calendar settings](#settings) ||
|#

### Parameter settings {#settings}

#|
|| **view**
[`string`](../data-types.md) | Default view for the calendar. Possible values:
- `day` — day
- `week` — week
- `month` — month
- `list` — list  ||
|| **meetSection**
[`string`](../data-types.md) | Calendar for invitations ||
|| **crmSection**
[`string`](../data-types.md) | Calendar for CRM ||
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
[`string`](../data-types.md) | Identifier of the calendar used when creating events if the calendar identifier is not provided in the parameters. 

Default value — `false` ||
|| **sendFromEmail**
[`string`](../data-types.md) | E-mail for sending mail invitations ||
|| **defaultSections**
[`object`](../data-types.md) | Settings for preset calendars.

The key of the settings object can be:
- `user[id]` — User Calendar type with user identifier. For example, `user12` corresponds to the user calendar with identifier `12`
- `group[id]` — Group Calendar type with group identifier. For example, `group36` corresponds to the group calendar with identifier `36`

The value of the object is the calendar identifier ||
|| **syncPeriodPast**
[`string`](../data-types.md) | Number of months for synchronization in the past period ||
|| **syncPeriodFuture**
[`string`](../data-types.md) | Number of months for synchronization in the future period ||
|| **defaultReminders**
[`object`](../data-types.md) | An object with standard [event reminder settings](#defaultReminders) ||
|#

### Object defaultReminders {#defaultReminders}

#|
|| **Name**
`type` | **Description** ||
|| **fullDay**
[`array`](../data-types.md) | Array of [standard reminder settings](#reminder-settings) for all-day events ||
|| **withTime**
[`array`](../data-types.md) | Array of [standard reminder settings](#reminder-settings) for timed events ||
|#

#### Reminder settings object {#reminder-settings}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../data-types.md) | Time type of the reminder. Possible values:
- `min` — minutes
- `hour` — hours
- `day` — days ||
|| **count**
[`integer`](../data-types.md) | Numeric value of the time interval ||
|#


## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

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

- JS

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

- PHP

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
                        }
                    ],
                    'withTime' => [
                        [
                            'type' => 'min',
                            'count' => 50
                        }
                    ]
                ]
            ]
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
[`boolean`](../data-types.md) | Returns `true` if the execution is successful ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "settings" for the method "calendar.user.settings.set" is not set"
}
```
{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "settings" for the method "calendar.user.settings.set" is not set | The required parameter `settings` is not provided ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-settings-get.md)
- [{#T}](./calendar-user-settings-get.md)
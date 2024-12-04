# Start a New Workday timeman.open

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.open` starts a new workday or resumes a closed or paused one.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_ID**
[`unknown`](../../data-types.md) | User identifier. Optional, defaults to the settings of the current user. ||
|| **TIME**
[`unknown`](../../data-types.md) | Start time of the workday. Optional, defaults to the current moment with the employee's saved time zone. The date must match the current calendar date. ||
|| **REPORT**^*^
[`unknown`](../../data-types.md) | Reason for changing the workday. Required when specifying the TIME parameter and when the employee's flexible schedule is disabled. ||
|| **LAT**
[`unknown`](../../data-types.md) | Geographic latitude at the start of the workday. Optional. ||
|| **LON**
[`unknown`](../../data-types.md) | Geographic longitude at the start of the workday. Optional. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

The time zone from the TIME parameter is considered the time zone of the current workday. The employee sees the time in this time zone, and the end time of the day is adjusted to this time zone, even if specified in another. The TIME parameter is accepted only if the workday is in the CLOSED status; in all other cases, an error will be returned.

## Example

{% list tabs %}

- cURL

    ```http
    https://account.bitrix24.com/rest/timeman.open?auth=xxxxxx&time=2017-04-21T07%3A30%3A00%2B08%3A00&report=Forgot&lat=54.7199881&lon=20.4879224&user_id=5
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{
    "result": {
        "STATUS": "OPENED",
        "TIME_START": "2017-04-21T07:30:00+08:00",
        "TIME_FINISH": null,
        "DURATION": "00:00:00",
        "TIME_LEAKS": "00:00:00",
        "ACTIVE": false,
        "IP_OPEN": "192.168.1.1",
        "IP_CLOSE": "",
        "LAT_OPEN": 54.7199881,
        "LON_OPEN": 20.4879224,
        "LAT_CLOSE": 0,
        "LON_CLOSE": 0,
        "TZ_OFFSET": 28800
    }
}
```

### Returned Data

#|
|| **Field** | **Description** | **Note** ||
|| **STATUS**
 [`string`](../../data-types.md) | Status of the current workday. | Possible values:
- OPENED - workday is ongoing
- CLOSED - workday is closed
- PAUSED - workday is paused
- EXPIRED - workday has expired (was opened before the start of the current calendar day and not closed) ||
|| **TIME_START**
[`datetime`](../../data-types.md) | Date-time of the start of the workday. | Time zone corresponds to the time zone of the start of the workday. ||
|| **TIME_FINISH**
[`datetime`](../../data-types.md) | Date-time of the end of the workday | Returns null for an unfinished workday. ||
|| **DURATION**
[`HH:MM:SS`](../../data-types.md) | Duration of the workday | Returns 00:00:00 for an unfinished workday. ||
|| **TIME_LEAKS**
[`HH:MM:SS`](../../data-types.md) | Total duration of breaks during the day | ||
|| **ACTIVE**
[`true`\|`false`](../../data-types.md) | Confirmation of the workday | A value of false means that the change to the workday is awaiting confirmation from the supervisor. ||
|| **IP_OPEN**
[`string`](../../data-types.md) | IP address from which the workday started | ||
|| **IP_CLOSE**
[`string`](../../data-types.md) | IP address from which the workday ended | Returns null for an unfinished workday. ||
|| **LAT_OPEN**
[`double`](../../data-types.md) | Geographic latitude of the point where the workday started | ||
|| **LON_OPEN**
[`double`](../../data-types.md) | Geographic longitude of the point where the workday started | ||
|| **LAT_CLOSE**
[`double`](../../data-types.md) | Geographic latitude of the point where the workday ended | ||
|| **LON_CLOSE**
[`double`](../../data-types.md) | Geographic longitude of the point where the workday ended | ||
|| **TZ_OFFSET**
[`int`](../../data-types.md) | Time zone offset of the employee | Implies the time zone in which the day started. When the day ends, the end time is adjusted to the time zone of the start of the day. ||
|| **TIME_FINISH_DEFAULT**
[`datetime`](../../data-types.md) | Default end time of the day | Displayed only for workdays in the EXPIRED status. "Recommended" end time value that can be shown to the user as a default value. ||
|#
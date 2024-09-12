# Start a New Work Day timeman.open

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

The method `timeman.open` starts a new work day or resumes a closed or paused one.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_ID**
[`unknown`](../../data-types.md) | User identifier. Optional; defaults to the settings of the current user. ||
|| **TIME**
[`unknown`](../../data-types.md) | Start time of the work day. Optional; defaults to the current moment with the employee's saved time zone. The date must match the current calendar date. ||
|| **REPORT**^*^
[`unknown`](../../data-types.md) | Reason for changing the work day. Required when the TIME parameter is specified and the employee's flexible schedule is disabled. ||
|| **LAT**
[`unknown`](../../data-types.md) | Geographic latitude of the start of the work day. Optional. ||
|| **LON**
[`unknown`](../../data-types.md) | Geographic longitude of the start of the work day. Optional. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

The time zone from the TIME parameter is considered the time zone of the current work day. The employee sees the time in this time zone, and the end time of the day is adjusted to this time zone, even if specified in another. The TIME parameter is accepted only if the work day is in the CLOSED status; in all other cases, an error will be returned.

## Example

Example call:

```http
https://account.bitrix24.com/rest/timeman.open?auth=xxxxxx&time=2017-04-21T07%3A30%3A00%2B08%3A00&report=Forgot&lat=54.7199881&lon=20.4879224&user_id=5
```

{% include [Example Note](../../../_includes/examples.md) %}

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
 [`string`](../../data-types.md) | Status of the current work day. | Possible values:
- OPENED - work day is ongoing
- CLOSED - work day is closed
- PAUSED - work day is paused
- EXPIRED - work day has expired (was opened before the start of the current calendar day and not closed) ||
|| **TIME_START**
[`datetime`](../../data-types.md) | Date-time of the start of the work day. | The time zone corresponds to the time zone of the start of the work day. ||
|| **TIME_FINISH**
[`datetime`](../../data-types.md) | Date-time of the end of the work day. | Returns null for an unfinished work day. ||
|| **DURATION**
[`HH:MM:SS`](../../data-types.md) | Duration of the work day. | Returns 00:00:00 for an unfinished work day. ||
|| **TIME_LEAKS**
[`HH:MM:SS`](../../data-types.md) | Total duration of breaks during the day. | ||
|| **ACTIVE**
[`true`\|`false`](../../data-types.md) | Confirmation of the work day. | A value of false means that the change to the work day is awaiting confirmation from the supervisor. ||
|| **IP_OPEN**
[`string`](../../data-types.md) | IP address from which the work day was started. | ||
|| **IP_CLOSE**
[`string`](../../data-types.md) | IP address from which the work day was completed. | Returns null for an unfinished work day. ||
|| **LAT_OPEN**
[`double`](../../data-types.md) | Geographic latitude of the point where the work day started. | ||
|| **LON_OPEN**
[`double`](../../data-types.md) | Geographic longitude of the point where the work day started. | ||
|| **LAT_CLOSE**
[`double`](../../data-types.md) | Geographic latitude of the point where the work day ended. | ||
|| **LON_CLOSE**
[`double`](../../data-types.md) | Geographic longitude of the point where the work day ended. | ||
|| **TZ_OFFSET**
[`int`](../../data-types.md) | Time zone offset of the employee. | Implies the time zone in which the day started. Upon completion of the day, the end time is adjusted to the time zone of the start of the day. ||
|| **TIME_FINISH_DEFAULT**
[`datetime`](../../data-types.md) | Default end time of the day. | Displayed only for work days in the EXPIRED status. "Recommended" end time that can be shown to the user as a default value. ||
|#
# Pause the Current Workday timeman.pause

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- examples missing
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.pause` pauses the current workday.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_ID**
[`unknown`](../../data-types.md) | User identifier. Optional; defaults to the settings of the current user. ||
|#

## Example

Example call:

```http
https://account.bitrix24.com/rest/timeman.pause?user_id=5
```

## Successful Response

> 200 OK
```json
{
    "result": {
        "STATUS": "PAUSED",
        "TIME_START": "2017-04-21T07:30:00+08:00",
        "TIME_FINISH": "2017-04-21T22:03:58+08:00",
        "DURATION": "00:00:00",
        "TIME_LEAKS": "00:00:00",
        "ACTIVE": false,
        "IP_OPEN": "192.168.0.1",
        "IP_CLOSE": "192.168.0.100",
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
[`datetime`](../../data-types.md) | Date-time when the workday started. | The time zone corresponds to the time zone of the start of the workday. ||
|| **TIME_FINISH**
[`datetime`](../../data-types.md) | Date-time when the workday ended. | Returns null for an unfinished workday. ||
|| **DURATION**
[`HH:MM:SS`](../../data-types.md) | Duration of the workday. | Returns 00:00:00 for an unfinished workday. ||
|| **TIME_LEAKS**
[`HH:MM:SS`](../../data-types.md) | Total duration of breaks during the day. | ||
|| **ACTIVE**
[`true`\|`false`](../../data-types.md) | Confirmation of the workday. | A value of false means that the change to the workday is awaiting confirmation from the supervisor. ||
|| **IP_OPEN**
[`string`](../../data-types.md) | IP address from which the workday started. | ||
|| **IP_CLOSE**
[`string`](../../data-types.md) | IP address from which the workday ended. | Returns null for an unfinished workday. ||
|| **LAT_OPEN**
[`double`](../../data-types.md) | Geographical latitude of the workday start point. | ||
|| **LON_OPEN**
[`double`](../../data-types.md) | Geographical longitude of the workday start point. | ||
|| **LAT_CLOSE**
[`double`](../../data-types.md) | Geographical latitude of the workday end point. | ||
|| **LON_CLOSE**
[`double`](../../data-types.md) | Geographical longitude of the workday end point. | ||
|| **TZ_OFFSET**
[`int`](../../data-types.md) | Time zone offset of the employee. | Implies the time zone in which the day started. When the day ends, the end time is adjusted to the time zone of the start of the day. ||
|| **TIME_FINISH_DEFAULT**
[`datetime`](../../data-types.md) | Default end time of the day. | Displayed only for workdays in the status EXPIRED. The "recommended" end time that can be shown to the user as a default value. ||
|#
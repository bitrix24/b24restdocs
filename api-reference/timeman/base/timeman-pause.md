# Pause the current workday timeman.pause

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

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

The `timeman.pause` method pauses the current workday.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_ID**
[`unknown`](../../data-types.md) | User identifier. Optional, defaults to the settings of the current user. ||
|#

## Example

{% list tabs %}

- cURL

    ```http
    https://account.bitrix24.com/rest/timeman.pause?user_id=5
    ```

{% endlist %}



## Successful response

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

### Returned data

#|
|| **Field** | **Description** | **Note** ||
|| **STATUS**
 [`string`](../../data-types.md) | Status of the current workday. | Possible values:
- OPENED - workday is ongoing
- CLOSED - workday is closed
- PAUSED - workday is paused
- EXPIRED - workday has expired (was opened before the start of the current calendar day and not closed) ||
|| **TIME_START**
[`datetime`](../../data-types.md) | Date-time when the workday started. | Time zone corresponds to the time zone of the start of the workday ||
|| **TIME_FINISH**
[`datetime`](../../data-types.md) | Date-time when the workday was completed | Returns null for an unfinished workday ||
|| **DURATION**
[`HH:MM:SS`](../../data-types.md) | Duration of the workday | Returns 00:00:00 for an unfinished workday ||
|| **TIME_LEAKS**
[`HH:MM:SS`](../../data-types.md) | Total duration of breaks during the day | ||
|| **ACTIVE**
[`true`\|`false`](../../data-types.md) | Confirmation of the workday | A value of false means that the change to the workday is awaiting confirmation from the supervisor. ||
|| **IP_OPEN**
[`string`](../../data-types.md) | IP address from which the workday started | ||
|| **IP_CLOSE**
[`string`](../../data-types.md) | IP address from which the workday was completed | Returns null for an unfinished workday ||
|| **LAT_OPEN**
[`double`](../../data-types.md) | Geographic latitude of the workday start point | ||
|| **LON_OPEN**
[`double`](../../data-types.md) | Geographic longitude of the workday start point | ||
|| **LAT_CLOSE**
[`double`](../../data-types.md) | Geographic latitude of the workday end point | ||
|| **LON_CLOSE**
[`double`](../../data-types.md) | Geographic longitude of the workday end point | ||
|| **TZ_OFFSET**
[`int`](../../data-types.md) | Time zone offset of the employee | Implies the time zone in which the day started. Upon completion of the day, the end time is adjusted to the time zone of the start of the day. ||
|| **TIME_FINISH_DEFAULT**
[`datetime`](../../data-types.md) | Default end time of the day | Displayed only for workdays in the EXPIRED status. The "recommended" end time value that can be shown to the user as a default value. ||
|#
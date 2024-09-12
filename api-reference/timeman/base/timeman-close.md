# Close the Current Day timeman.close

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.close` ends the current workday.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_ID**
[`unknown`](../../data-types.md) | User identifier. Optional; defaults to the settings of the current user. ||
|| **TIME**
[`unknown`](../../data-types.md) | End time of the workday. Optional; by default, the workday closes at the current moment in the timezone where the workday started. The date must match the date the workday began. ||
|| **REPORT**^*^
[`unknown`](../../data-types.md) | Reason for changing the workday. Required when specifying the TIME parameter and when the employee's flexible schedule is disabled. ||
|| **LAT**
[`unknown`](../../data-types.md) | Geographic latitude at the start of the workday. Optional. ||
|| **LON**
[`unknown`](../../data-types.md) | Geographic longitude at the start of the workday. Optional. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

If the timezone of the end time of the workday differs from the timezone of the start of the workday, the time will be converted to the timezone of the start of the day.

## Example

Example call:

```http
https://account.bitrix24.com/rest/timeman.close?auth=xxxxx&time=2017-04-21T17%3A30%3A00%2B09%3A00&report=test&lat=52.5162434&lon=13.3774829&user_id=5
```

{% include [Example Notes](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{
    "result": {
        "STATUS": "CLOSED",
        "TIME_START": "2017-04-21T07:30:00+08:00",
        "TIME_FINISH": "2017-04-21T16:30:00+08:00",
        "DURATION": "09:00:00",
        "TIME_LEAKS": "00:00:00",
        "ACTIVE": false,
        "IP_OPEN": "192.168.0.1",
        "IP_CLOSE": "192.168.0.100",
        "LAT_OPEN": 54.7199881,
        "LON_OPEN": 20.4879224,
        "LAT_CLOSE": 52.5162434,
        "LON_CLOSE": 13.3774829,
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
- EXPIRED - workday has expired (was opened before the start of the current calendar day and was not closed) ||
|| **TIME_START**
[`datetime`](../../data-types.md) | Date-time of the start of the workday. | The timezone corresponds to the timezone of the start of the workday. ||
|| **TIME_FINISH**
[`datetime`](../../data-types.md) | Date-time of the end of the workday. | Returns null for an unfinished workday. ||
|| **DURATION**
[`HH:MM:SS`](../../data-types.md) | Duration of the workday. | Returns 00:00:00 for an unfinished workday. ||
|| **TIME_LEAKS**
[`HH:MM:SS`](../../data-types.md) | Total duration of breaks during the day. | ||
|| **ACTIVE**
[`true`\|`false`](../../data-types.md) | Confirmation of the workday. | A value of false means that the change to the workday is awaiting confirmation from the supervisor. ||
|| **IP_OPEN**
[`string`](../../data-types.md) | IP address from which the workday started. | ||
|| **IP_CLOSE**
[`string`](../../data-types.md) | IP address from which the workday was closed. | Returns null for an unfinished workday. ||
|| **LAT_OPEN**
[`double`](../../data-types.md) | Geographic latitude of the point where the workday started. | ||
|| **LON_OPEN**
[`double`](../../data-types.md) | Geographic longitude of the point where the workday started. | ||
|| **LAT_CLOSE**
[`double`](../../data-types.md) | Geographic latitude of the point where the workday ended. | ||
|| **LON_CLOSE**
[`double`](../../data-types.md) | Geographic longitude of the point where the workday ended. | ||
|| **TZ_OFFSET**
[`int`](../../data-types.md) | Timezone offset of the employee. | Implies the timezone in which the day started. When closing the day, the end time is adjusted to the timezone of the start of the day. ||
|| **TIME_FINISH_DEFAULT**
[`datetime`](../../data-types.md) | Default end time of the day. | Displayed only for workdays in the status EXPIRED. The "recommended" end time value that can be shown to the user as a default value. ||
|#
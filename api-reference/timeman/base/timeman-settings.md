# Get User Work Time Settings timeman.settings

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- examples missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.settings` returns the work time settings for the user.


## Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_ID**
[`unknown`](../../data-types.md) | User identifier. Optional; by default, the settings for the current user are returned. ||
|#

## Example

Example call:

```http
https://account.bitrix24.com/rest/timeman.settings/?auth=xxxxxx&user_id=1
```
{% include [Note on examples](../../../_includes/examples.md) %}

## Successful Response

> 200 OK
```json
{
    "result": {
        "ADMIN": true,
        "UF_TIMEMAN": true,
        "UF_TM_ALLOWED_DELTA": "00:15:00",
        "UF_TM_FREE": false,
        "UF_TM_MAX_START": "09:15:00",
        "UF_TM_MIN_DURATION": "08:00:00",
        "UF_TM_MIN_FINISH": "17:45:00"
    }
}
```

### Returned Data

#|
|| **Field** | **Description** | **Note** ||
|| **ADMIN**
[`true`\|`false`](../../data-types.md) | Indicates whether the user has rights to manage others' workdays | Returned only for the current user ||
|| **UF_TIMEMAN**
[`true`\|`false`](../../data-types.md) | Indicates whether work time tracking is enabled for the user | ||
|| **UF_TM_FREE**
[`true`\|`false`](../../data-types.md) | Indicates whether the user has a flexible work schedule | When flexible scheduling is enabled, changes to work time do not require confirmation or a reason. ||
|| **UF_TM_MAX_START**
[`HH:MM:SS`](../../data-types.md) | The maximum start time for the workday set for the user | A workday starting later than the specified time will be considered a violation. ||
|| **UF_TM_MIN_FINISH**
[`HH:MM:SS`](../../data-types.md) | The minimum finish time for the workday set for the user | A workday finishing earlier than the specified time will be considered a violation. ||
|| **UF_TM_MIN_DURATION**
[`HH:MM:SS`](../../data-types.md) | The minimum duration of the workday set for the user | A workday with a duration shorter than the specified will be considered a violation. ||
|| **UF_TM_ALLOWED_DELTA**
[`HH:MM:SS`](../../data-types.md) | The allowed time interval for changing work time set for the user | Changing the workday by an interval shorter than the specified will not require confirmation from the supervisor. ||
|#
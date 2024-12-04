# Get User Work Time Settings timeman.settings

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

The method `timeman.settings` returns the work time settings of the user.


## Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_ID**
[`unknown`](../../data-types.md) | User identifier. Optional, by default returns settings for the current user. ||
|#

## Example

{% list tabs %}

- cURL

    ```http
    https://account.bitrix24.com/rest/timeman.settings/?auth=xxxxxx&user_id=1
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in case of success

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
[`true`\|`false`](../../data-types.md) | Does the user have rights to manage others' workdays | Returned only for the current user ||
|| **UF_TIMEMAN**
[`true`\|`false`](../../data-types.md) | Is work time tracking enabled for the user | ||
|| **UF_TM_FREE**
[`true`\|`false`](../../data-types.md) | Is a flexible work schedule enabled for the user | When a flexible schedule is enabled, changes to work time do not require confirmation or reason ||
|| **UF_TM_MAX_START**
[`HH:MM:SS`](../../data-types.md) | The maximum start time of the workday set for the user | A workday starting later than the specified time will be considered a violation ||
|| **UF_TM_MIN_FINISH**
[`HH:MM:SS`](../../data-types.md) | The minimum finish time of the workday set for the user | A workday finishing earlier than the specified time will be considered a violation ||
|| **UF_TM_MIN_DURATION**
[`HH:MM:SS`](../../data-types.md) | The minimum duration of the workday set for the user | A workday with a duration shorter than specified will be considered a violation ||
|| **UF_TM_ALLOWED_DELTA**
[`HH:MM:SS`](../../data-types.md) | The allowed time delta for changing work time set for the user | Changing the workday by a period shorter than specified will not require confirmation from the supervisor ||
|#
# Get User Work Time Settings timeman.settings

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.settings` retrieves the work time settings for a user.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID**
[`integer`](../../data-types.md) | User identifier.

By default — the identifier of the current user ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":503}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.settings
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":503,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.settings
    ```

- JS

    ```js
    BX24.callMethod(
        'timeman.settings',
        {
            'USER_ID' : 503
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.settings',
        [
            'USER_ID' => 503
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
    "result": {
        "UF_TIMEMAN": true,
        "UF_TM_FREE": false,
        "UF_TM_MAX_START": "09:15:00",
        "UF_TM_MIN_FINISH": "17:45:00",
        "UF_TM_MIN_DURATION": "08:00:00",
        "UF_TM_ALLOWED_DELTA": "00:15:00",
        "ADMIN": true
    },
    "time": {
        "start": 1742994169.701416,
        "finish": 1742994169.7355511,
        "duration": 0.03413510322570801,
        "processing": 0.0076749324798583984,
        "date_start": "2025-03-26T16:02:49+02:00",
        "date_finish": "2025-03-26T16:02:49+02:00",
        "operating_reset_at": 1742994769,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response.

Contains an object with the description of the user's work time settings ||
|| **UF_TIMEMAN**
[`boolean`](../../data-types.md) | Whether work time tracking is enabled for the user.

Has a value of `true` if enabled ||
|| **UF_TM_FREE**
[`boolean`](../../data-types.md) | Whether the user has a flexible work schedule.

Has a value of `true` if enabled.

A user with a flexible schedule does not need to confirm changes to work time with a supervisor or provide reasons for changes ||
|| **UF_TM_MAX_START**
[`string`](../../data-types.md) | Maximum start time of the workday in `HH:MM:SS` format.

Starting the workday later than the set time is considered a violation ||
|| **UF_TM_MIN_FINISH**
[`string`](../../data-types.md) | Minimum finish time of the workday in `HH:MM:SS` format.

Finishing the workday earlier than the set time is considered a violation ||
|| **UF_TM_MIN_DURATION**
[`string`](../../data-types.md) | Minimum duration of the workday in `HH:MM:SS` format.

A workday with a duration shorter than the set time is considered a violation ||
|| **UF_TM_ALLOWED_DELTA**
[`string`](../../data-types.md) | Allowed time change for work hours in `HH:MM:SS` format.

Changing the workday by a period shorter than the set time does not require supervisor confirmation ||
|| **ADMIN**
[`boolean`](../../data-types.md) | Whether the user can manage the workdays of other employees. Returned only for the current user.

Has a value of `true` if permissions are granted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"",
    "error_description":"User not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| empty string | User not found | User with the specified `USER_ID` not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-open.md)
- [{#T}](./timeman-pause.md)
- [{#T}](./timeman-close.md)
- [{#T}](./timeman-status.md)
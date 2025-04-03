# Start a New Workday timeman.open

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.open` starts a new workday or resumes the workday after a break or completion.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID**
[`integer`](../../data-types.md) | User identifier. 

Defaults to the current user ||
|| **TIME**
[`datetime`](../../data-types.md) | The start date and time of the workday in the [ATOM](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) (ISO-8601) format, for example, `2025-02-12T15:52:01+00:00`. The date must match the current calendar date. 

The `TIME` parameter can be passed if the workday is in the `CLOSED` status. In other cases, an error will be returned.

By default, the workday opens with the current time in the employee's time zone.

The system takes into account the time zone specified in the parameter and considers it the user's time zone ||
|| **REPORT**
[`string`](../../data-types.md) | Reason for changing the workday.

Required under the conditions:
- the `TIME` parameter is specified
- the employee does not have a flexible schedule ||
|| **LAT**
[`double`](../../data-types.md) | Geographic latitude of the start of the workday ||
|| **LON**
[`double`](../../data-types.md) | Geographic longitude of the start of the workday ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":503,"TIME":"2025-03-27T08:00:01+00:00","REPORT":"I forgot to start the workday","LAT":53.548841,"LON":9.987274}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.open
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":503,"TIME":"2025-03-27T08:00:01+00:00","REPORT":"I forgot to start the workday","LAT":53.548841,"LON":9.987274,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.open
    ```

- JS

    ```js
    BX24.callMethod(
        'timeman.open',
        {
            'USER_ID' : 503,
            'TIME': '2025-03-27T08:00:01+00:00',
            'REPORT': 'I forgot to start the workday',
            'LAT': 53.548841, 
            'LON': 9.987274
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
        'timeman.open',
        [
            'USER_ID' => 503,
            'TIME' => '2025-03-27T08:00:01+00:00',
            'REPORT' => 'I forgot to start the workday',
            'LAT' => 53.548841,
            'LON' => 9.987274
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
        "STATUS": "OPENED",
        "TIME_START": "2025-03-27T08:00:01+02:00",
        "TIME_FINISH": null,
        "DURATION": "00:00:00",
        "TIME_LEAKS": "00:00:00",
        "ACTIVE": false,
        "IP_OPEN": "",
        "IP_CLOSE": "",
        "LAT_OPEN": 53.548841000000003,
        "LON_OPEN": 9.9872739999999993,
        "LAT_CLOSE": 0,
        "LON_CLOSE": 0,
        "TZ_OFFSET": 7200
    },
    "time": {
        "start": 1743056587.6559751,
        "finish": 1743056587.8529301,
        "duration": 0.19695496559143066,
        "processing": 0.16714906692504883,
        "date_start": "2025-03-27T09:23:07+03:00",
        "date_finish": "2025-03-27T09:23:07+03:00",
        "operating_reset_at": 1743057187,
        "operating": 0.1671299934387207
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response.

Contains an object with the description of the workday ||
|| **STATUS**
 [`string`](../../data-types.md) | Status of the current workday.
 
 Possible values:
- `OPENED` — opened
- `CLOSED` — closed
- `PAUSED` — paused
- `EXPIRED` — expired, meaning opened until the start of the current calendar day and not closed ||
|| **TIME_START**
[`datetime`](../../data-types.md) | Date and time the workday started.

The time zone corresponds to the time zone of the start of the workday ||
|| **TIME_FINISH**
[`datetime`](../../data-types.md) | Date and time the workday was completed.

Returns `null` for an unfinished workday ||
|| **DURATION**
[`string`](../../data-types.md) | Duration of the workday in the format `HH:MM:SS`.

Returns `00:00:00` for an unfinished workday ||
|| **TIME_LEAKS**
[`string`](../../data-types.md) | Total duration of breaks during the day in the format `HH:MM:SS` ||
|| **ACTIVE**
[`boolean`](../../data-types.md) | Confirmation of the workday.

A value of `false` means that the change to the workday is awaiting confirmation from the supervisor ||
|| **IP_OPEN**
[`string`](../../data-types.md) | IP address from which the workday was started ||
|| **IP_CLOSE**
[`string`](../../data-types.md) | IP address from which the workday was completed.

Returns `null` for an unfinished workday ||
|| **LAT_OPEN**
[`double`](../../data-types.md) | Geographic latitude of the point where the workday started ||
|| **LON_OPEN**
[`double`](../../data-types.md) | Geographic longitude of the point where the workday started ||
|| **LAT_CLOSE**
[`double`](../../data-types.md) | Geographic latitude of the point where the workday was completed ||
|| **LON_CLOSE**
[`double`](../../data-types.md) | Geographic longitude of the point where the workday was completed ||
|| **TZ_OFFSET**
[`integer`](../../data-types.md) | Time zone offset of the employee in which the workday started.

The completion time of the workday is adjusted to the time zone of the start of the day ||
|| **TIME_FINISH_DEFAULT**
[`datetime`](../../data-types.md) | Recommended value for the end of the day, which can be displayed to the user as a default value.

Displayed only for workdays in the expired status `EXPIRED` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the time taken to process the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"WRONG_DATETIME",
    "error_description":"Day open date should correspond to the current date"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| empty string | User not found | User with the specified `USER_ID` not found ||
|| `WRONG_DATETIME` | Day open date should correspond to the current date | The date of opening the workday must match the current calendar date ||
|| `TIME` | Unable to set time, work day is paused | Cannot pass the `TIME` parameter for a paused workday ||

|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-pause.md)
- [{#T}](./timeman-close.md)
- [{#T}](./timeman-status.md)
- [{#T}](./timeman-settings.md)
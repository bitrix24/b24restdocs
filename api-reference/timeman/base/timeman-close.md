# Close Current Day timeman.close

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.close` ends the current workday.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID**
[`integer`](../../data-types.md) | User identifier. 

Defaults to the current user ||
|| **TIME**
[`datetime`](../../data-types.md) | The end time and date of the workday in the [ATOM](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) (ISO-8601) format, for example, `2025-02-12T15:52:01+00:00`. The date must match the start date of the workday.

By default, the workday is closed at the current moment in the timezone where the workday was started.

If the end timezone differs from the start timezone, the end time is automatically converted to the timezone in which the day was started. ||
|| **REPORT**
[`string`](../../data-types.md) | Reason for changing the workday.

Required under the conditions:
- the `TIME` parameter is specified
- the employee does not have a flexible schedule ||
|| **LAT**
[`double`](../../data-types.md) | Geographic latitude of the end of the workday ||
|| **LON**
[`double`](../../data-types.md) | Geographic longitude of the end of the workday ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":503,"TIME":"2025-03-27T17:00:01+00:00","REPORT":"Forgot to close the workday","LAT":53.548841,"LON":9.987274}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/timeman.close
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":503,"TIME":"2025-03-27T17:00:01+00:00","REPORT":"Forgot to close the workday","LAT":53.548841,"LON":9.987274,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/timeman.close
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'timeman.close',
    		{
    			'USER_ID' : 503,
    			'TIME': '2025-03-27T17:00:01+00:00',
    			'REPORT': 'Forgot to close the workday',
    			'LAT': 53.548841, 
    			'LON': 9.987274
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.close',
                [
                    'USER_ID' => 503,
                    'TIME'    => '2025-03-27T17:00:01+00:00',
                    'REPORT'  => 'Forgot to close the workday',
                    'LAT'     => 53.548841,
                    'LON'     => 9.987274,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error closing timeman: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'timeman.close',
        {
            'USER_ID' : 503,
            'TIME': '2025-03-27T17:00:01+00:00',
            'REPORT': 'Forgot to close the workday',
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.close',
        [
            'USER_ID' => 503,
            'TIME' => '2025-03-27T17:00:01+00:00',
            'REPORT' => 'Forgot to close the workday',
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

HTTP Status: **200**

```json
{
    "result": {
        "STATUS": "CLOSED",
        "TIME_START": "2025-03-27T08:00:01+02:00",
        "TIME_FINISH": "2025-03-27T19:00:01+02:00",
        "DURATION": "09:37:04",
        "TIME_LEAKS": "01:22:56",
        "ACTIVE": false,
        "IP_OPEN": "",
        "IP_CLOSE": "83.219.151.30",
        "LAT_OPEN": 53.548841000000003,
        "LON_OPEN": 9.9872739999999993,
        "LAT_CLOSE": 53.548841000000003,
        "LON_CLOSE": 9.9872739999999993,
        "TZ_OFFSET": 7200
    },
    "time": {
        "start": 1743057653.725821,
        "finish": 1743057654.0894129,
        "duration": 0.36359190940856934,
        "processing": 0.3278491497039795,
        "date_start": "2025-03-27T09:40:53+03:00",
        "date_finish": "2025-03-27T09:40:54+03:00",
        "operating_reset_at": 1743058253,
        "operating": 0.32782983779907227
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response.

Contains an object describing the workday ||
|| **STATUS**
 [`string`](../../data-types.md) | Status of the current workday.
 
 Possible values:
- `OPENED` — opened
- `CLOSED` — closed
- `PAUSED` — paused
- `EXPIRED` — expired, meaning opened before the start of the current calendar day and not closed ||
|| **TIME_START**
[`datetime`](../../data-types.md) | Date and time the workday started.

The timezone corresponds to the timezone of the start of the workday ||
|| **TIME_FINISH**
[`datetime`](../../data-types.md) | Date and time the workday was closed.

Returns `null` for an unfinished workday ||
|| **DURATION**
[`string`](../../data-types.md) | Duration of the workday in `HH:MM:SS` format.

Returns `00:00:00` for an unfinished workday ||
|| **TIME_LEAKS**
[`string`](../../data-types.md) | Total duration of breaks during the day in `HH:MM:SS` format. ||
|| **ACTIVE**
[`boolean`](../../data-types.md) | Confirmation of the workday.

A value of `false` means that the change to the workday is awaiting confirmation from the supervisor ||
|| **IP_OPEN**
[`string`](../../data-types.md) | IP address from which the workday started ||
|| **IP_CLOSE**
[`string`](../../data-types.md) | IP address from which the workday was closed.

Returns `null` for an unfinished workday ||
|| **LAT_OPEN**
[`double`](../../data-types.md) | Geographic latitude of the point where the workday started ||
|| **LON_OPEN**
[`double`](../../data-types.md) | Geographic longitude of the point where the workday started ||
|| **LAT_CLOSE**
[`double`](../../data-types.md) | Geographic latitude of the point where the workday was closed ||
|| **LON_CLOSE**
[`double`](../../data-types.md) | Geographic longitude of the point where the workday was closed ||
|| **TZ_OFFSET**
[`integer`](../../data-types.md) | Timezone offset of the employee in which the workday was started.

The end time of the workday is adjusted to the timezone of the start of the day ||
|| **TIME_FINISH_DEFAULT**
[`datetime`](../../data-types.md) | Recommended value for the end of the day, which can be presented to the user as a default value.

Displayed only for workdays in the expired status `EXPIRED` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the time taken to process the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"WRONG_DATETIME",
    "error_description":"Day close date should correspond to the day open date"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| empty string | User not found | User with the specified `USER_ID` not found ||
|| `WRONG_DATETIME` | Day close date should correspond to the day open date | The closing date of the workday must match the opening date ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./timeman-open.md)
- [{#T}](./timeman-pause.md)
- [{#T}](./timeman-status.md)
- [{#T}](./timeman-settings.md)
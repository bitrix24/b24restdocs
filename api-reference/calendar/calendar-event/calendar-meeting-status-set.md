# Set Participation Status for the Current User calendar.meeting.status.set

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sets the participation status for the current user's event.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **eventId***  
[`integer`](../../data-types.md) | The identifier of the event.

You can obtain the identifier using the [calendar.event.get](./calendar-event-get.md) or [calendar.event.get.nearest](./calendar-event-get-nearest.md) methods. ||
|| **status***  
[`string`](../../data-types.md) | The participation status for the event. Possible values: 
- `Y` — accepted
- `N` — declined
- `Q` — invited, but not yet responded ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"eventId":651,"status":"Y"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.meeting.status.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"eventId":651,"status":"Y","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.meeting.status.set
    ```

- JS

    ```js
    BX24.callMethod(
        'calendar.meeting.status.set',
        {
            eventId: 651,
            status: 'Y'
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.meeting.status.set',
        [
            'eventId' => 651,
            'status' => 'Y'
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
    "result": true,
    "time": {
        "start": 1728732695.718461,
        "finish": 1728732696.029712,
        "duration": 0.3112509250640869,
        "processing": 0.051657915115356445,
        "date_start": "2024-12-10T11:31:35+00:00",
        "date_finish": "2024-12-10T11:31:36+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Success of setting the status.

Returns `true` if the status is set successfully. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "status" for the method "calendar.meeting.status.set" is not set."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "status" for the method "calendar.meeting.status.set" is not set. | The required parameter `status` was not provided. ||
|| Empty string | The required parameter "eventId" for the method "calendar.meeting.status.set" is not set. | The required parameter `eventId` was not provided. ||
|| Empty string | Invalid value for the "status" parameter | The `status` parameter contains a value other than `Q`, `Y`, or `N`. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-meeting-status-get.md)
# Get the Participation Status of the Current User in the Event calendar.meeting.status.get

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves the participation status of the current user in an event.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **eventId***
[`integer`](../../data-types.md) | Identifier of the event.

You can obtain the identifier using the [calendar.event.get](./calendar-event-get.md) or [calendar.event.get.nearest](./calendar-event-get-nearest.md) methods ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"eventId":651}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.meeting.status.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"eventId":651,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.meeting.status.get
    ```

- JS

    ```js
    BX24.callMethod(
        'calendar.meeting.status.get',
        {
            eventId: 651
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.meeting.status.get',
        [
            'eventId' => 651
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
    "result": "Y",
    "time": {
        "start": 1728732695.718461,
        "finish": 1728732696.029712,
        "duration": 0.256943941116333,
        "processing": 0.03064703941345215,
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
[`string`](../../data-types.md) | Participation status of the current user. Possible values:
- `Y` — accepted
- `N` — declined
- `Q` — invited, but not yet responded
 ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "An error occurred while retrieving the status"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "eventId" for the method "calendar.meeting.status.set" is not set | The required parameter `eventId` was not provided ||
|| Empty string | An error occurred while retrieving the status | Another error ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-meeting-status-set.md)
- [{#T}](./calendar-accessibility-get.md)
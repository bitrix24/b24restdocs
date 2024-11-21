# Unbind Registered Event Handler event.unbind

> Who can execute the method: administrator

The method `event.unbind` cancels the registration of an event handler.

It only works when logged in as a user with administrative rights to the account.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../data-types.md) | Event name ||
|| **handler***
[`string`](../data-types.md) | Link to the event handler ||
|| **auth_type**
[`integer`](../data-types.md) | User identifier under which the event handler is authorized.

{% note info %}

If you need to remove event handlers set with an empty `auth_type` (authorized on behalf of the user who triggered the event), but keep other handlers, specify `auth_type=0` or an empty value for the parameter.

{% endnote %} 
||
|| **event_type**
[`string`](../data-types.md) | Values: `online\|offline`. By default, `event_type=online`, and the method's behavior remains unchanged. If `event_type=offline` is called, the method works with [offline events](https://dev.1c-bitrix.com/learning/course/index.php?COURSE_ID=99&CHAPTER_ID=04462&LESSON_PATH=8771.5380.2461.4462) ||
|#

If any parameters are not specified, all event handlers that meet the other requirements will be removed.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "event": "ONCRMLEADADD",
        "handler": "https://www.my-domain.com/handler/"
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/event.unbind
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "event": "ONCRMLEADADD",
        "handler": "https://www.my-domain.com/handler/",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/event.unbind
    ```

- JS

    ```js
    BX24.callUnbind(
        'ONCRMLEADADD',
        'https://www.my-domain.com/handler/',
        15,
        (result) => console.log(result)
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'event.unbind',
        [
            'EVENT' => 'ONCRMLEADADD',
            'HANDLER' => 'https://www.my-domain.com/handler/',
            'AUTH_TYPE' => 15
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- B24-PHP-SDK

    ```php        
    try {
        $eventCode = 'your_event_code'; // Replace with your actual event code
        $handlerUrl = 'https://your.handler.url'; // Replace with your actual handler URL
        $userId = null; // Replace with your actual user ID or leave as null
        $result = $serviceBuilder
            ->getMainScope()
            ->event()
            ->unbind($eventCode, $handlerUrl, $userId);
        print($result->getUnbindedHandlersCount());
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The method returns the number of event handlers removed upon invocation.

```json
{
    "result": {
        "count": 1
    },
    "time": {
        "start": 1721298360.468008,
        "finish": 1721298360.553977,
        "duration": 0.0859689712524414,
        "processing": 0.0023431777954101562,
        "date_start": "2024-07-18T12:26:00+02:00",
        "date_finish": "2024-07-18T12:26:00+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## See Also

- [{#T}](../bx24-js-sdk/how-to-call-rest-methods/bx24-call-unbind.md)

## Continue Learning

- [{#T}](./events.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
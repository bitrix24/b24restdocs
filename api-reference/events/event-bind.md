# Register a New Event Handler event.bind

> Who can execute the method: any user

The method `event.bind` registers a new event handler.

The method works only in the context of [application](../app-installation/index.md) authorization. It can operate under a user with portal administration rights as well as under a regular user. The method for a user without administrator rights is available with limitations:

1. Offline events are not available; attempting to set them will raise an exception.
2. Events are set on behalf of the current user (see the description of the `auth_type` parameter). Explicitly specifying an `auth_type` different from the current user's `ID` will also raise an exception.

{% note info %}

Since requests will come from Bitrix servers, any URL must be accessible for GET/POST requests from the outside.

{% endnote %}

The interface for this method is [BX24.callBind](../bx24-js-sdk/how-to-call-rest-methods/bx24-call-bind.md).

{% note info %}

When an application is deleted or updated, its actions will be removed. Therefore, in the installer of each version, they need to be set from scratch.

{% endnote %}

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
[`integer`](../data-types.md) | User ID under which the event handler is authorized. By default, the authorization of the user whose actions triggered the event will be used ||
|| **event_type**
[`string`](../data-types.md) | Values: `online\|offline`. By default, `event_type=online`, and the method's behavior does not change. If `event_type=offline` is called, the method works with [offline events](./offline-events.md) ||
|| **auth_connector**
[`string`](../data-types.md) | Source key. This parameter is intended for [offline events](./offline-events.md). It allows excluding false event triggers ||
|| **options**
[`string`](../data-types.md) | Additional settings for the registered event, if any ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

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
    https://**put_your_bitrix24_address**/rest/event.bind
    ```

- JS

    ```js
    BX24.callBind(
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
        'event.bind',
        [
            'event' => 'ONCRMLEADADD',
            'handler' => 'https://www.my-domain.com/handler/',
            'auth_type' => 15
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
    "result": true,
    "time": {
        "start": 1721296536.908506,
        "finish": 1721296537.007365,
        "duration": 0.09885907173156738,
        "processing": 0.03251290321350098,
        "date_start": "2024-07-18T11:55:36+02:00",
        "date_finish": "2024-07-18T11:55:37+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Success of execution ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_EVENT_NOT_FOUND",
    "error_description":"Event not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ERROR_EVENT_NOT_FOUND` | Event not found | The event specified is incorrect ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./events.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
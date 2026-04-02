# Subscribe to Events im.v2.Event.subscribe

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute the method: authorized user

The method `im.v2.Event.subscribe` subscribes the current user to event logging. After subscribing, message events are recorded in the log and become accessible through [im.v2.Event.get](./event-get.md).

The method is idempotent: repeated calls are safe and do not result in an error.

## Method Parameters

The method does not accept any parameters.

## Code Examples

{% include [Examples Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.v2.Event.subscribe
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.v2.Event.subscribe
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.v2.Event.subscribe', {});

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.v2.Event.subscribe',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: ' . print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.v2.Event.subscribe',
        {},
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call('im.v2.Event.subscribe', []);

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Subscribed';
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": true,
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | `true` if subscription is successful ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Get User Events im.v2.Event.get](./event-get.md)
- [Unsubscribe from Events im.v2.Event.unsubscribe](./event-unsubscribe.md)
- [Event Formats im.v2](./events.md)
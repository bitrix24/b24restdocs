# Responding to Notifications with Quick Response im.notify.answer

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.answer` sends a text quick response to a notification.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **NOTIFY_ID*** 
[`integer`](../../data-types.md) | The identifier of the notification that supports a quick response.

You can obtain the notification identifier using the [im.notify.get](./im-notify-get.md) method. ||
|| **ANSWER_TEXT*** 
[`string`](../../data-types.md) | The text of the quick response. ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"NOTIFY_ID":270,"ANSWER_TEXT":"I will participate"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.answer
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"NOTIFY_ID":270,"ANSWER_TEXT":"I will participate","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.answer
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.notify.answer', {
        NOTIFY_ID: 270,
        ANSWER_TEXT: 'I will participate',
      });

      const { result } = response.getData();
      console.log(result);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.notify.answer',
            [
                'NOTIFY_ID' => 270,
                'ANSWER_TEXT' => 'I will participate',
            ]
        );

        $result = $response->getResponseData()->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            var_dump($result->data());
        }
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.notify.answer',
        {
            NOTIFY_ID: 270,
            ANSWER_TEXT: 'I will participate',
        },
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

    $result = CRest::call(
        'im.notify.answer',
        [
            'NOTIFY_ID' => 270,
            'ANSWER_TEXT' => 'I will participate',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```
{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "result_message": [
            "Your response has been successfully sent."
        ]
    },
    "time": {
        "start": 1760000000.0,
        "finish": 1760000000.1,
        "duration": 0.1,
        "processing": 0.04,
        "date_start": "2026-03-02T09:30:00+01:00",
        "date_finish": "2026-03-02T09:30:00+01:00",
        "operating_reset_at": 1760030000,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object containing the result of the response submission. 

The structure of the object is described in detail [below](#result-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

### Result Object {#result-object}

#|
|| **Name**
`Type` | **Description** ||
|| **result_message**
[`array`](../../data-types.md) 
[`boolean`](../../data-types.md) | The response from the quick response handler. It may contain an array of strings or `false`. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ANSWER_TEXT_ERROR",
    "error_description": "ANSWER_TEXT can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ID_ERROR` | Notification ID can't be empty | The `NOTIFY_ID` parameter is not provided or is `<= 0`. ||
|| `ANSWER_TEXT_ERROR` | ANSWER_TEXT can't be empty | The `ANSWER_TEXT` parameter is empty. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-notify.md)
- [{#T}](./im-notify-personal-add.md)
- [{#T}](./im-notify-system-add.md)
- [{#T}](./im-notify-get.md)
- [{#T}](./im-notify-schema-get.md)
- [{#T}](./im-notify-read-list.md)
- [{#T}](./im-notify-read.md)
- [{#T}](./im-notify-read-all.md)
- [{#T}](./im-notify-confirm.md)
- [{#T}](./im-notify-delete.md)
- [{#T}](./im-notify-history-search.md)
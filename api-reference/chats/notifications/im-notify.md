# Send Notification im.notify

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `im.notify` method sends a notification to a user.

{% note info "" %}

This method is only available when called through the application.

{% endnote %}

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **USER_ID*** 
[`integer`](../../data-types.md) | The identifier of the user receiving the notification.

You can obtain the user ID using the [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md) methods. ||
|| **TYPE**
[`string`](../../data-types.md) | The type of notification. 

Allowed values:
- `USER` — personal notification
- `SYSTEM` — system notification
 
Default value is `USER` ||
|| **MESSAGE*** 
[`string`](../../data-types.md) | The text of the notification. The method trims whitespace from the edges of the string before sending. ||
|| **MESSAGE_OUT**
[`string`](../../data-types.md) | The text of the notification for external channels, such as email. ||
|| **TAG**
[`string`](../../data-types.md) | A unique tag for the notification within the application. When adding a notification with an existing tag, other notifications will be removed. Pass it with `CLIENT_ID` when calling via webhook. ||
|| **SUB_TAG**
[`string`](../../data-types.md) | An additional notification tag without uniqueness checks. Pass it with `CLIENT_ID` when calling via webhook. ||
|| **ATTACH**
[`object`](../../data-types.md) 
[`string`](../../data-types.md) | An attachment for the notification in object format or JSON string. For more details, see the [Attachments](../messages/attachments/index.md) section. ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_ID":5,"TYPE":"USER","MESSAGE":"Reminder","MESSAGE_OUT":"Reminder (email)","TAG":"TASK_42","SUB_TAG":"TASK|42"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_ID":5,"TYPE":"SYSTEM","MESSAGE":"System message","MESSAGE_OUT":"System message (email)","TAG":"SYSTEM_42","SUB_TAG":"SYSTEM|42","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.notify', {
        USER_ID: 5,
        TYPE: 'USER',
        MESSAGE: 'Reminder',
        MESSAGE_OUT: 'Reminder (email)',
        TAG: 'TASK_42',
        SUB_TAG: 'TASK|42',
      });

      const { result } = response.getData();
      console.log('Notification ID:', result);
    } catch (error) {
      console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.notify',
            [
                'USER_ID' => 5,
                'TYPE' => 'USER',
                'MESSAGE' => 'Reminder',
                'MESSAGE_OUT' => 'Reminder (email)',
                'TAG' => 'TASK_42',
                'SUB_TAG' => 'TASK|42',
            ]
        );

        $result = $response->getResponseData()->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Notification ID: ' . $result->data();
        }
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.notify',
        {
            USER_ID: 5,
            TYPE: 'USER',
            MESSAGE: 'Reminder',
            MESSAGE_OUT: 'Reminder (email)',
            TAG: 'TASK_42',
            SUB_TAG: 'TASK|42',
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
        'im.notify',
        [
            'USER_ID' => 5,
            'TYPE' => 'USER',
            'MESSAGE' => 'Reminder',
            'MESSAGE_OUT' => 'Reminder (email)',
            'TAG' => 'TASK_42',
            'SUB_TAG' => 'TASK|42',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Notification ID: ' . $result['result'];
    }
    ```
{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": 12345,
    "time": {
        "start": 1760000000.0,
        "finish": 1760000000.1,
        "duration": 0.1,
        "processing": 0.04,
        "date_start": "2026-03-03T10:00:00+01:00",
        "date_finish": "2026-03-03T10:00:00+01:00",
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
[`integer`](../../data-types.md) 
[`boolean`](../../data-types.md) | The identifier of the created notification. If the notification was not created, it may return `false`. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "USER_ID_EMPTY",
    "error_description": "User ID can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Access for this method not allowed by session authorization. | The method was called with session authorization, which is prohibited. ||
|| `USER_ID_EMPTY` | User ID can't be empty | The `USER_ID` parameter was not provided, or `USER_ID <= 0`. ||
|| `MESSAGE_EMPTY` | Message can't be empty | The message text was not provided. ||
|| `ATTACH_OVERSIZE` | You have exceeded the maximum allowable size of attach | The maximum allowable size of the `ATTACH` is exceeded — 30 KB. ||
|| `ATTACH_ERROR` | Incorrect attach params | An incorrect format for the `ATTACH` was provided. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-notify-personal-add.md)
- [{#T}](./im-notify-system-add.md)
- [{#T}](./im-notify-get.md)
- [{#T}](./im-notify-schema-get.md)
- [{#T}](./im-notify-read-list.md)
- [{#T}](./im-notify-read.md)
- [{#T}](./im-notify-read-all.md)
- [{#T}](./im-notify-answer.md)
- [{#T}](./im-notify-confirm.md)
- [{#T}](./im-notify-delete.md)
- [{#T}](./im-notify-history-search.md)
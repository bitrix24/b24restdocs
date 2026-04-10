# Get Notifications im.notify.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.get` returns a list of user notifications in parts. The next part is requested using `LAST_ID` and `LAST_TYPE`.

Notifications are sorted first by descending creation date, then by descending identifiers.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **LAST_ID**
[`integer`](../../data-types.md) | Identifier of the last notification from the previous page to load the next one. Typically taken from the `id` field of the last element in the `notifications` array in the response of the previous step or in the response of [im.notify.history.search](./im-notify-history-search.md)

The notification identifier is also returned by the methods [im.notify](./im-notify.md), [im.notify.personal.add](./im-notify-personal-add.md), and [im.notify.system.add](./im-notify-system-add.md).

This parameter should be used together with `LAST_TYPE` ||
|| **LAST_TYPE**
[`integer`](../../data-types.md) | Technical pagination cursor.

Allowed values: 
- `1` — continue fetching from the confirmations stage
- `3` — continue fetching from the regular notifications stage

This parameter should be used together with `LAST_ID`.

For values other than `1` and `3`, the server does not return a separate error. ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of notifications per page. Default value is `50`. Maximum value is `50`. ||
|| **CONVERT_TEXT**
[`string`](../../data-types.md) | Convert notification text. Value `Y` enables conversion, any other value disables it. ||
|#

On a single page, the method can return a mixed set of notifications: first confirmations, then regular notifications to reach `LIMIT`.

For stable pagination:

- pass `LAST_ID` of the last element from the previous page
- use `LAST_TYPE` from the previous pagination step

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"LAST_ID":1500,"LAST_TYPE":3,"LIMIT":20,"CONVERT_TEXT":"Y"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"LAST_ID":1500,"LAST_TYPE":3,"LIMIT":20,"CONVERT_TEXT":"Y","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.notify.get', {
        LAST_ID: 1500,
        LAST_TYPE: 3,
        LIMIT: 20,
        CONVERT_TEXT: 'Y',
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
            'im.notify.get',
            [
                'LAST_ID' => 1500,
                'LAST_TYPE' => 3,
                'LIMIT' => 20,
                'CONVERT_TEXT' => 'Y',
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
        'im.notify.get',
        {
            LAST_ID: 1500,
            LAST_TYPE: 3,
            LIMIT: 20,
            CONVERT_TEXT: 'Y',
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
        'im.notify.get',
        [
            'LAST_ID' => 1500,
            'LAST_TYPE' => 3,
            'LIMIT' => 20,
            'CONVERT_TEXT' => 'Y',
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
        "total_count": 120,
        "total_unread_count": 5,
        "chat_id": 77,
        "notifications": [
            {
                "id": 1500,
                "chat_id": 77,
                "author_id": 1,
                "date": "2026-03-03T09:00:00+00:00",
                "notify_type": 2,
                "notify_module": "rest",
                "notify_event": "rest_notify",
                "notify_tag": "MP|12345|TASK_42",
                "notify_sub_tag": "MP|12345|TASK|42",
                "notify_title": "",
                "setting_name": "rest|rest_notify",
                "text": "Reminder",
                "notify_read": "N",
                "params": null
            }
        ],
        "users": [
            {
                "id": 1,
                "active": true,
                "name": "John Smith",
                "first_name": "John",
                "last_name": "Smith",
                "work_position": "",
                "color": "#1eb4aa",
                "avatar": "https://example.bitrix24.com/upload/main/avatar.png",
                "avatar_hr": "https://example.bitrix24.com/upload/main/avatar.png",
                "gender": "M",
                "birthday": "15-05",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "external_auth_id": "socservices",
                "status": "online",
                "idle": false,
                "last_activity_date": "2026-03-03T15:04:20+02:00",
                "mobile_last_date": false,
                "desktop_last_date": false,
                "absent": false,
                "departments": [1],
                "phones": {
                    "work_phone": "+11234567890",
                    "inner_phone": "22"
                },
                "bot_data": null,
                "type": "user",
                "website": "example.com",
                "email": "john@example.com"
            }
        ]
    },
    "time": {
        "start": 1760000000.0,
        "finish": 1760000000.1,
        "duration": 0.1,
        "processing": 0.04,
        "date_start": "2026-03-03T10:00:00+02:00",
        "date_finish": "2026-03-03T10:00:00+02:00",
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
[`object`](../../data-types.md) | Object containing notification data. 

The structure of the object is described in detail [below](#result-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Result Object {#result-object}

#|
|| **Name**
`Type` | **Description** ||
|| **total_count**
[`integer`](../../data-types.md) | Total number of notifications ||
|| **total_unread_count**
[`integer`](../../data-types.md) | Number of unread notifications ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the system notification chat ||
|| **notifications**
[`array`](../../data-types.md) | List of notifications. 

The structure of the object is described in detail [below](#notification-object) ||
|| **users**
[`array`](../../data-types.md) | Array of objects containing data about notification authors.

The structure of the object is described in detail [below](#users-object) ||
|#

If the user does not have a system notification chat or it contains no messages, the method returns only the `notifications` and `users` fields.

#### Notification Object {#notification-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the notification ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the system notification chat ||
|| **author_id**
[`integer`](../../data-types.md) | Identifier of the notification author ||
|| **date**
[`string`](../../data-types.md) | Date and time of the notification in ISO 8601 format (RFC3339) ||
|| **notify_type**
[`integer`](../../data-types.md) | Type of notification ||
|| **notify_module**
[`string`](../../data-types.md) | Identifier of the notification module ||
|| **notify_event**
[`string`](../../data-types.md) | Code of the notification event ||
|| **notify_tag**
[`string`](../../data-types.md) | Notification tag ||
|| **notify_sub_tag**
[`string`](../../data-types.md) | Additional notification tag ||
|| **notify_title**
[`string`](../../data-types.md) | Title of the notification ||
|| **setting_name**
[`string`](../../data-types.md) | Code of the setting in the format ```MODULE|EVENT``` ||
|| **text**
[`string`](../../data-types.md) | Text of the notification ||
|| **notify_read**
[`string`](../../data-types.md) | Read status of the notification: `Y` or `N` ||
|| **notify_buttons**
[`string`](../../data-types.md) | JSON keyboard for confirmation-type notifications. This field is not always present ||
|| **params**
[`object`](../../data-types.md) 
[`null`](../../data-types.md) | Additional parameters of the notification ||
|#

#### Users Object {#users-object}

{% include [User Object Tables](../_includes/user-object-tables.md) %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "LAST_ID_AND_LAST_TYPE",
    "error_description": "Parameters LAST_ID and LAST_TYPE should be used together."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `LAST_ID_AND_LAST_TYPE` | Parameters LAST_ID and LAST_TYPE should be used together | Only one parameter from the pair `LAST_ID` and `LAST_TYPE` was provided ||
|| `LAST_ID_STRING` | Last notification ID can't be string | The `LAST_ID` parameter contains a non-numeric value ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-notify.md)
- [{#T}](./im-notify-personal-add.md)
- [{#T}](./im-notify-system-add.md)
- [{#T}](./im-notify-schema-get.md)
- [{#T}](./im-notify-read-list.md)
- [{#T}](./im-notify-read.md)
- [{#T}](./im-notify-read-all.md)
- [{#T}](./im-notify-answer.md)
- [{#T}](./im-notify-confirm.md)
- [{#T}](./im-notify-delete.md)
- [{#T}](./im-notify-history-search.md)
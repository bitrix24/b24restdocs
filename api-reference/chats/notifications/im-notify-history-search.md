# Searching Notification History im.notify.history.search

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.history.search` performs a search through the user's notification history.

Notifications are sorted first by descending creation date, then by descending identifiers.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **SEARCH_TEXT**
[`string`](../../data-types.md) | Search text. If `SEARCH_TYPE` and `SEARCH_DATE` are not specified, the string length must be at least `3` characters ||
|| **SEARCH_TYPE**
[`string`](../../data-types.md) | Filter by notification type in the format `MODULE` or ```MODULE|EVENT```. Ready values can be obtained using the method [im.notify.schema.get](./im-notify-schema-get.md) ||
|| **SEARCH_TYPES**
[`array`](../../data-types.md) | Array of filters by notification types in the format `MODULE` or ```MODULE|EVENT``` ||
|| **SEARCH_DATE**
[`string`](../../data-types.md) | Date filter in ISO 8601 format (RFC3339) ||
|| **SEARCH_DATE_FROM**
[`string`](../../data-types.md) | Start of the date range in ISO 8601 format (RFC3339). Used together with `SEARCH_DATE_TO` ||
|| **SEARCH_DATE_TO**
[`string`](../../data-types.md) | End of the date range in ISO 8601 format (RFC3339). Used together with `SEARCH_DATE_FROM` ||
|| **SEARCH_AUTHORS**
[`array`](../../data-types.md) | Array of notification author identifiers for filtering ||
|| **LAST_ID**
[`integer`](../../data-types.md) | Identifier of the last notification from the previous page to load the next one. Usually taken from the `id` field of the last element in the `notifications` array in the response of the previous search step or in the response of [im.notify.get](./im-notify-get.md) ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of notifications per page. Default value is `50`. Maximum value is `50` ||
|| **CONVERT_TEXT**
[`string`](../../data-types.md) | Convert notification text. Value `Y` enables conversion, any other value disables it ||
|| **GROUP_TAG**
[`string`](../../data-types.md) | Group tag for notifications for additional filtering ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SEARCH_TEXT":"invoice","SEARCH_TYPE":"tasks|task_update","SEARCH_DATE":"2026-03-03T16:52:29+01:00","LAST_ID":1500,"LIMIT":20,"CONVERT_TEXT":"Y","GROUP_TAG":"TASK|42"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.history.search
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SEARCH_TEXT":"invoice","SEARCH_TYPE":"tasks|task_update","SEARCH_DATE":"2026-03-03T16:52:29+01:00","LAST_ID":1500,"LIMIT":20,"CONVERT_TEXT":"Y","GROUP_TAG":"TASK|42","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.history.search
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NotifyHistorySearchResult = {
      chat_id: number
      total_results: number
      notifications: Array<{
        id: number
        chat_id: number
        author_id: number
        date: ISODate | null
        notify_type: number
        notify_module: string
        notify_event: string
        notify_tag: string
        notify_sub_tag: string
        notify_title: string
        setting_name: string
        text: string
        notify_read: string
        params: object | null
      }>
      users: Array<{
        id: number
        active: boolean
        name: string
        first_name: string
        last_name: string
        work_position: string
        color: string
        avatar: string
        avatar_hr: string
        gender: string
        birthday: string
        extranet: boolean
        network: boolean
        bot: boolean
        connector: boolean
        external_auth_id: string
        status: string
        idle: ISODate | false
        last_activity_date: ISODate | null
        mobile_last_date: ISODate | false
        desktop_last_date: ISODate | false
        absent: ISODate | false
        departments: number[]
        phones: { work_phone?: string; inner_phone?: string } | false
        bot_data: object | null
        type: string
        website: string
        email: string
      }>
    }

    try {
      const response = await $b24.actions.v2.call.make<NotifyHistorySearchResult>({
        method: 'im.notify.history.search',
        params: {
          SEARCH_TEXT: 'invoice',
          SEARCH_TYPE: 'tasks|task_update',
          SEARCH_DATE: '2026-03-03T16:52:29+03:00',
          LAST_ID: 1500,
          LIMIT: 20,
          CONVERT_TEXT: 'Y',
          GROUP_TAG: 'TASK|42',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(
          result.chat_id,
          result.total_results,
          result.notifications.length,
        )
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function searchNotifyHistory() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.notify.history.search',
            params: {
              SEARCH_TEXT: 'invoice',
              SEARCH_TYPE: 'tasks|task_update',
              SEARCH_DATE: '2026-03-03T16:52:29+03:00',
              LAST_ID: 1500,
              LIMIT: 20,
              CONVERT_TEXT: 'Y',
              GROUP_TAG: 'TASK|42',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(
            result.chat_id,
            result.total_results,
            result.notifications.length,
          )
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchNotifyHistory)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.notify.history.search',
            [
                'SEARCH_TEXT' => 'invoice',
                'SEARCH_TYPE' => 'tasks|task_update',
                'SEARCH_DATE' => '2026-03-03T16:52:29+01:00',
                'LAST_ID' => 1500,
                'LIMIT' => 20,
                'CONVERT_TEXT' => 'Y',
                'GROUP_TAG' => 'TASK|42',
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
        'im.notify.history.search',
        {
            SEARCH_TEXT: 'invoice',
            SEARCH_TYPE: 'tasks|task_update',
            SEARCH_DATE: '2026-03-03T16:52:29+01:00',
            LAST_ID: 1500,
            LIMIT: 20,
            CONVERT_TEXT: 'Y',
            GROUP_TAG: 'TASK|42',
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
        'im.notify.history.search',
        [
            'SEARCH_TEXT' => 'invoice',
            'SEARCH_TYPE' => 'tasks|task_update',
            'SEARCH_DATE' => '2026-03-03T16:52:29+01:00',
            'LAST_ID' => 1500,
            'LIMIT' => 20,
            'CONVERT_TEXT' => 'Y',
            'GROUP_TAG' => 'TASK|42',
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
        "chat_id": 77,
        "total_results": 1,
        "notifications": [
            {
                "id": 1500,
                "chat_id": 77,
                "author_id": 1,
                "date": "2026-03-03T09:00:00+00:00",
                "notify_type": 2,
                "notify_module": "tasks",
                "notify_event": "task_update",
                "notify_tag": "TASK|42",
                "notify_sub_tag": "",
                "notify_title": "",
                "setting_name": "tasks|task_update",
                "text": "invoice",
                "notify_read": "Y",
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
                "last_activity_date": "2026-03-04T15:04:20+01:00",
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
        "date_start": "2026-03-04T16:00:00+01:00",
        "date_finish": "2026-03-04T16:00:00+01:00",
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
[`object`](../../data-types.md) | Object containing the search result. 

The structure of the object is described in detail [below](#result-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Result Object {#result-object}

#|
|| **Name**
`Type` | **Description** ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the system notification chat ||
|| **total_results**
[`integer`](../../data-types.md) | Total number of search results. This field is returned only on the first page of the search ||
|| **notifications**
[`array`](../../data-types.md) | List of notifications.

The structure of the object is described in detail [below](#notification-object) ||
|| **users**
[`array`](../../data-types.md) | Users who authored the notifications

The structure of the object is described in detail [below](#user-object) ||
|#

### Notification Object {#notification-object}

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

### User Object {#user-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the user ||
|| **active**
[`boolean`](../../data-types.md) | User's active status ||
|| **name**
[`string`](../../data-types.md) | User's full name ||
|| **first_name**
[`string`](../../data-types.md) | User's first name ||
|| **last_name**
[`string`](../../data-types.md) | User's last name ||
|| **work_position**
[`string`](../../data-types.md) | User's position ||
|| **color**
[`string`](../../data-types.md) | User's color in hex format ||
|| **avatar**
[`string`](../../data-types.md) | Link to the avatar ||
|| **avatar_hr**
[`string`](../../data-types.md) | Link to the high-resolution avatar ||
|| **gender**
[`string`](../../data-types.md) | User's gender ||
|| **birthday**
[`string`](../../data-types.md) | Birthday in `DD-MM` format or an empty string ||
|| **extranet**
[`boolean`](../../data-types.md) | External user status ||
|| **network**
[`boolean`](../../data-types.md) | Bitrix24 Network user status ||
|| **bot**
[`boolean`](../../data-types.md) | Bot status ||
|| **connector**
[`boolean`](../../data-types.md) | Open Channels user status ||
|| **external_auth_id**
[`string`](../../data-types.md) | External authorization code ||
|| **status**
[`string`](../../data-types.md) | User's status ||
|| **idle**
[`datetime`](../../data-types.md) | User's idle date or `false` ||
|| **last_activity_date**
[`datetime`](../../data-types.md) | User's last activity date ||
|| **mobile_last_date**
[`datetime`](../../data-types.md) | User's last activity date in the mobile application or `false` ||
|| **desktop_last_date**
[`datetime`](../../data-types.md) | User's last activity date in the desktop application or `false` ||
|| **absent**
[`datetime`](../../data-types.md) | User's absence end date or `false` ||
|| **departments**
[`array`](../../data-types.md) | Array of department identifiers ||
|| **phones**
[`object`](../../data-types.md) | User's phones or `false` [(detailed description)](#phones) ||
|| **bot_data**
[`object`](../../data-types.md) | Bot data or `null` ||
|| **type**
[`string`](../../data-types.md) | User type ||
|| **website**
[`string`](../../data-types.md) | User's website ||
|| **email**
[`string`](../../data-types.md) | User's email ||
|#

#### Phones Object {#phones}

#|
|| **Name**
`Type` | **Description** ||
|| **personal_mobile**
[`string`](../../data-types.md) | Mobile phone ||
|| **inner_phone**
[`string`](../../data-types.md) | Internal phone ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "SEARCH_TEXT_ERROR",
    "error_description": "SEARCH_TEXT can't be less than 3 symbols"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `SEARCH_TEXT_ERROR` | SEARCH_TEXT can't be less than 3 symbols | This error occurs if `SEARCH_TYPE`, `SEARCH_DATE`, `SEARCH_TYPES`, `SEARCH_AUTHORS`, the pair `SEARCH_DATE_FROM` and `SEARCH_DATE_TO` are not specified, and the length of `SEARCH_TEXT` is less than `3` ||
|| `LAST_ID_STRING` | Last notification ID can't be string | The `LAST_ID` parameter contains a non-numeric value ||
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
- [{#T}](./im-notify-answer.md)
- [{#T}](./im-notify-confirm.md)
- [{#T}](./im-notify-delete.md)
# Get Notifications im.notify.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.get` returns the current user's notifications page by page.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **LAST_ID**
[`integer`](../../data-types.md) | Identifier of the last notification on the previous page — the `id` field of the last element in the `notifications` array. The method returns notifications with an identifier less than `LAST_ID`.

Do not pass it for the first page. Pass it together with `LAST_TYPE`, otherwise the method returns the `LAST_ID_AND_LAST_TYPE` error. The value must contain digits only, otherwise the method returns the `LAST_ID_STRING` error ||
|| **LAST_TYPE**
[`integer`](../../data-types.md) | Type of the last notification on the previous page. Allowed values:
- `1` — the last notification requires confirmation, its `notify_type` field equals `1`
- `3` — the last notification is a regular one, its `notify_type` field equals `2` or `4`

Do not pass it for the first page. Pass it together with `LAST_ID`.

If you pass `1` when the last notification was a regular one, the method returns regular notifications from the beginning of the list, and they are repeated. For other values, the method returns an empty `notifications` array ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of notifications per page. Default is `50`, maximum is `50`. The method reduces a larger value to `50`.

Do not pass `0` or negative values: the method will not return regular notifications ||
|| **CONVERT_TEXT**
[`string`](../../data-types.md) | The method accepts the parameter but ignores it ||
|#

### How to Retrieve All Notifications {#pagination}

The method sorts notifications by descending creation date, then by descending `id`. Notifications that require confirmation go on the page first, then the method fills the rest of the page with regular notifications.

1. Request the first page without `LAST_ID` and `LAST_TYPE`
2. Take the last element of the `notifications` array. Pass its `id` in `LAST_ID`. If its `notify_type` equals `1`, pass `LAST_TYPE` = `1`, otherwise `LAST_TYPE` = `3`
3. Repeat step 2 until the method returns fewer notifications than `LIMIT`

The response does not contain a ready-made cursor for the next page: calculate `LAST_ID` and `LAST_TYPE` from the last element.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"LAST_ID":1500,"LAST_TYPE":3,"LIMIT":20}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"LAST_ID":1500,"LAST_TYPE":3,"LIMIT":20,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type NotificationItem = {
      id: number
      chat_id: number
      author_id: number
      date: ISODate
      notify_type: number
      notify_module: string
      notify_event: string
      notify_tag: string
      notify_sub_tag: string
      notify_title: string
      setting_name: string
      text: string
      params: object | null
      notify_read: 'Y' | 'N'
      notify_buttons?: string
    }

    type UserItem = {
      id: number
      active: boolean
      name: string
      first_name: string
      last_name: string
      work_position: string | null
      color: string
      avatar: string
      avatar_hr: string
      gender: 'M' | 'F'
      birthday: string
      extranet: boolean
      network: boolean
      bot: boolean
      connector: boolean
      external_auth_id: string
      internal_account: boolean
      intranet_user: boolean
      status: string
      idle: ISODate | false
      last_activity_date: ISODate | false
      mobile_last_date: ISODate | false
      desktop_last_date: ISODate | false
      absent: ISODate | false
      departments: number[]
      phones: Record<string, string> | false
      bot_data: object | null
      type: string
      website: string
      email: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    // total_count, total_unread_count and chat_id are absent when the user has no notifications
    type ImNotifyGetResult = {
      total_count?: number
      total_unread_count?: number
      chat_id?: number
      notifications: NotificationItem[]
      users: UserItem[]
    }

    try {
      const response = await $b24.actions.v2.call.make<ImNotifyGetResult>({
        method: 'im.notify.get',
        params: {
          LAST_ID: 1500,
          LAST_TYPE: 3,
          LIMIT: 20,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Notifications on page:', result.notifications.length)

        // Cursor for the next page is built from the last notification
        const last = result.notifications.at(-1)
        if (last && result.notifications.length === 20) {
          console.info('Next page:', { LAST_ID: last.id, LAST_TYPE: last.notify_type === 1 ? 1 : 3 })
        }
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
      async function getNotifications() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.notify.get',
            params: {
              LAST_ID: 1500,
              LAST_TYPE: 3,
              LIMIT: 20,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Notifications on page:', result.notifications.length)

          // Cursor for the next page is built from the last notification
          const last = result.notifications.at(-1)
          if (last && result.notifications.length === 20) {
            console.info('Next page:', { LAST_ID: last.id, LAST_TYPE: last.notify_type === 1 ? 1 : 3 })
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getNotifications)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.notify.get(
            last_id=1500,
            last_type=3,
            limit=20,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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
            ]
        );

        $result = $response->getResponseData()->getResult();

        print_r($result);
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
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.notify.get", b24.Params{
    	"LAST_ID":   1500,
    	"LAST_TYPE": 3,
    	"LIMIT":     20,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("im.notify.get: %w", err)
    }

    var item struct {
    	TotalCount       int `json:"total_count"`
    	TotalUnreadCount int `json:"total_unread_count"`
    	ChatID           int `json:"chat_id"`
    	Notifications    []struct {
    		ID         int    `json:"id"`
    		NotifyType int    `json:"notify_type"`
    		Text       string `json:"text"`
    	} `json:"notifications"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.TotalCount, item.TotalUnreadCount, len(item.Notifications))

    // The next page cursor is built from the last notification
    if n := len(item.Notifications); n == 20 {
    	last := item.Notifications[n-1]
    	lastType := 3
    	if last.NotifyType == 1 {
    		lastType = 1
    	}
    	fmt.Println("LAST_ID:", last.ID, "LAST_TYPE:", lastType)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "total_count": 34,
        "total_unread_count": 13,
        "chat_id": 2535,
        "notifications": [
            {
                "id": 1499,
                "chat_id": 2535,
                "author_id": 0,
                "date": "2026-11-10T14:30:11+01:00",
                "notify_type": 4,
                "notify_module": "rest",
                "notify_event": "rest_notify",
                "notify_tag": "",
                "notify_sub_tag": "",
                "notify_title": "",
                "setting_name": "rest|rest_notify",
                "text": "Invoice [B]#42[/B] has been paid",
                "params": {
                    "ATTACH": [
                        {
                            "ID": 1794317528,
                            "BLOCKS": [
                                {
                                    "MESSAGE": "Amount: 15,000 EUR"
                                }
                            ],
                            "DESCRIPTION": "",
                            "COLOR": "#29619b",
                            "COLOR_TOKEN": "base"
                        }
                    ]
                },
                "notify_read": "N"
            },
            {
                "id": 1497,
                "chat_id": 2535,
                "author_id": 15,
                "date": "2026-11-10T14:29:54+01:00",
                "notify_type": 2,
                "notify_module": "rest",
                "notify_event": "rest_notify",
                "notify_tag": "",
                "notify_sub_tag": "",
                "notify_title": "",
                "setting_name": "rest|rest_notify",
                "text": "Check the contract by the end of the day",
                "params": null,
                "notify_read": "N"
            }
        ],
        "users": [
            {
                "id": 15,
                "active": true,
                "name": "Klaus Weber",
                "first_name": "Klaus",
                "last_name": "Weber",
                "work_position": null,
                "color": "#58cc47",
                "avatar": "https://cdn.bitrix24.com/b1234567/main/avatar.png",
                "avatar_hr": "https://cdn.bitrix24.com/b1234567/main/avatar.png",
                "gender": "M",
                "birthday": "",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "external_auth_id": "socservices",
                "internal_account": true,
                "intranet_user": false,
                "status": "online",
                "idle": false,
                "last_activity_date": "2026-11-10T14:28:38+01:00",
                "mobile_last_date": false,
                "desktop_last_date": false,
                "absent": false,
                "departments": [],
                "phones": {
                    "personal_mobile": "4915112345678"
                },
                "bot_data": null,
                "type": "user",
                "website": "",
                "email": ""
            }
        ]
    },
    "time": {
        "start": 1794317458,
        "finish": 1794317458.62455,
        "duration": 0.6245501041412354,
        "processing": 0,
        "date_start": "2026-11-10T14:30:58+01:00",
        "date_finish": "2026-11-10T14:30:58+01:00",
        "operating_reset_at": 1794317940,
        "operating": 0.120635986328125
    }
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with notifications and their authors [(detailed description)](#result-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result-object}

#|
|| **Name**
`Type` | **Description** ||
|| **total_count**
[`integer`](../../data-types.md) | Total number of the user's notifications ||
|| **total_unread_count**
[`integer`](../../data-types.md) | Number of unread notifications ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the user's system notification chat ||
|| **notifications**
[`array`](../../data-types.md) | Array of notifications on the page [(detailed description)](#notification-object) ||
|| **users**
[`array`](../../data-types.md) | Array of notification authors on the page [(detailed description)](#users-object). System notifications with `author_id` = `0` are not included in the array ||
|#

If the user has no notifications, `result` contains only the empty `notifications` and `users` arrays.

#### Notification Object {#notification-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the notification. Use the `id` of the last element as `LAST_ID` for the next page ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the user's system notification chat ||
|| **author_id**
[`integer`](../../data-types.md) | Identifier of the notification author. For system notifications — `0` ||
|| **date**
[`datetime`](../../data-types.md) | Date and time the notification was created ||
|| **notify_type**
[`integer`](../../data-types.md) | Type of notification. Possible values:
- `1` — notification with buttons, requires confirmation
- `2` — personal notification from a user
- `4` — system notification

Use the value of the last element to choose `LAST_TYPE` for the next page ||
|| **notify_module**
[`string`](../../data-types.md) | Identifier of the module that sent the notification ||
|| **notify_event**
[`string`](../../data-types.md) | Code of the notification event ||
|| **notify_tag**
[`string`](../../data-types.md) | Notification tag or an empty string ||
|| **notify_sub_tag**
[`string`](../../data-types.md) | Additional notification tag or an empty string ||
|| **notify_title**
[`string`](../../data-types.md) | Title of the notification or an empty string ||
|| **setting_name**
[`string`](../../data-types.md) | Notification type code in the format ```MODULE|EVENT```. The list of codes is returned by the [im.notify.schema.get](./im-notify-schema-get.md) method ||
|| **text**
[`string`](../../data-types.md) | Text of the notification. Formatting is passed as BB codes, for example `[B]...[/B]` ||
|| **params**
[`object`](../../data-types.md) | Additional parameters of the notification, for example an `ATTACH` attachment, or `null` ||
|| **notify_read**
[`string`](../../data-types.md) | Read status of the notification. Possible values: `Y` — read, `N` — unread ||
|| **notify_buttons**
[`string`](../../data-types.md) | JSON string with the notification buttons. The field is present only in notifications with `notify_type` = `1` ||
|#

#### Users Object {#users-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the user ||
|| **active**
[`boolean`](../../data-types.md) | User's activity status ||
|| **name**
[`string`](../../data-types.md) | User's full name ||
|| **first_name**
[`string`](../../data-types.md) | User's first name ||
|| **last_name**
[`string`](../../data-types.md) | User's last name ||
|| **work_position**
[`string`](../../data-types.md) | User's position or `null` ||
|| **color**
[`string`](../../data-types.md) | User's color in hex format ||
|| **avatar**
[`string`](../../data-types.md) | Link to the avatar or an empty string ||
|| **avatar_hr**
[`string`](../../data-types.md) | Link to the high-resolution avatar or an empty string ||
|| **gender**
[`string`](../../data-types.md) | User's gender. Possible values: `M` — male, `F` — female ||
|| **birthday**
[`string`](../../data-types.md) | Birthday in `DD-MM` format or an empty string ||
|| **extranet**
[`boolean`](../../data-types.md) | Extranet user status ||
|| **network**
[`boolean`](../../data-types.md) | Bitrix24 Network user status ||
|| **bot**
[`boolean`](../../data-types.md) | Bot status ||
|| **connector**
[`boolean`](../../data-types.md) | Open Channels user status ||
|| **external_auth_id**
[`string`](../../data-types.md) | External authorization code ||
|| **internal_account**
[`boolean`](../../data-types.md) | Internal account flag: `external_auth_id` does not belong to external authorization types ||
|| **intranet_user**
[`boolean`](../../data-types.md) | Employee flag: the user has a department and an internal account ||
|| **status**
[`string`](../../data-types.md) | User's status ||
|| **idle**
[`boolean`](../../data-types.md) | Always `false` in this method ||
|| **last_activity_date**
[`datetime`](../../data-types.md) | User's last activity date or `false` ||
|| **mobile_last_date**
[`boolean`](../../data-types.md) | Always `false` in this method ||
|| **desktop_last_date**
[`boolean`](../../data-types.md) | Always `false` in this method ||
|| **absent**
[`datetime`](../../data-types.md) | User's absence end date or `false` ||
|| **departments**
[`array`](../../data-types.md) | Array of department identifiers ||
|| **phones**
[`object`](../../data-types.md) | User's phones [(detailed description)](#phones) or `false` if the user has no phones ||
|| **bot_data**
[`object`](../../data-types.md) | Bot data or `null` ||
|| **type**
[`string`](../../data-types.md) | User type. Possible values: `user`, `bot`, `extranet`, `collaber`, `guest` ||
|| **website**
[`string`](../../data-types.md) | User's website ||
|| **email**
[`string`](../../data-types.md) | User's email ||
|#

#### Phones Object {#phones}

The object contains only the phones that are filled in.

#|
|| **Name**
`Type` | **Description** ||
|| **work_phone**
[`string`](../../data-types.md) | Work phone ||
|| **personal_mobile**
[`string`](../../data-types.md) | Mobile phone ||
|| **inner_phone**
[`string`](../../data-types.md) | Internal phone ||
|#

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
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `LAST_ID_AND_LAST_TYPE` | Parameters LAST_ID and LAST_TYPE should be used together | Only one parameter from the pair `LAST_ID` and `LAST_TYPE` was passed ||
|| `400` | `LAST_ID_STRING` | Last notification ID can't be string | The `LAST_ID` value contains characters other than digits, for example `abc` or `-5` ||
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
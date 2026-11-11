# Searching Notification History im.notify.history.search

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.history.search` searches the current user's notifications by text, type, date, and author.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **SEARCH_TEXT**
[`string`](../../data-types.md) | Text to search for in the notification text. The search is case-insensitive.

If no other filters are passed, the length without leading and trailing spaces must be at least `3` characters ||
|| **SEARCH_TYPE**
[`string`](../../data-types.md) | Notification type in the format `MODULE` or ```MODULE|EVENT```, for example `calendar` or ```calendar|change```. Type codes are returned by the [im.notify.schema.get](./im-notify-schema-get.md) method.

Ignored if `SEARCH_TYPES` is passed ||
|| **SEARCH_TYPES**
[`array`](../../data-types.md) | Array of notification types, for example `["calendar", "tasks"]`.

If the array contains one element, the ```MODULE|EVENT``` format is allowed. If there are several elements, pass only `MODULE` module codes: in this case, ```MODULE|EVENT``` values find nothing ||
|| **SEARCH_DATE**
[`datetime`](../../data-types.md) | Date and time in RFC 3339 format, for example `2026-11-10T00:00:00+01:00`. The method finds notifications created within 24 hours from this moment.

Ignored if `SEARCH_DATE_FROM` and `SEARCH_DATE_TO` are passed ||
|| **SEARCH_DATE_FROM**
[`datetime`](../../data-types.md) | Start of the period in RFC 3339 format. Pass it together with `SEARCH_DATE_TO`.

If you pass only `SEARCH_DATE_FROM` without other filters, the method returns an empty `result` array ||
|| **SEARCH_DATE_TO**
[`datetime`](../../data-types.md) | End of the period in RFC 3339 format. Pass it together with `SEARCH_DATE_FROM`. The method adds 24 hours to the value: with `2026-11-10T00:00:00+01:00`, the selection includes notifications for November 10 ||
|| **SEARCH_AUTHORS**
[`array`](../../data-types.md) | Array of notification author identifiers, for example `[1, 15]`. The value `0` selects system notifications.

You can retrieve user identifiers with the [user.get](../../user/user-get.md) method ||
|| **GROUP_TAG**
[`string`](../../data-types.md) | Notification group for additional filtering. Allowed values:
- `confirm` — notifications that require confirmation
- `mention` — mentions
- `admin` — notifications for the administrator
- numeric identifier of a custom notification group

Works only together with other filters ||
|| **LAST_ID**
[`integer`](../../data-types.md) | Identifier of the last notification on the previous page — the `id` field of the last element in the `notifications` array. The method returns notifications with an identifier less than `LAST_ID`.

Do not pass it for the first page. The value must contain digits only, otherwise the method returns the `LAST_ID_STRING` error ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of notifications per page. Default is `50`, maximum is `50`. The method reduces a larger value to `50`.

Do not pass `0` or negative values: the method will return all found notifications without a limit ||
|| **CONVERT_TEXT**
[`string`](../../data-types.md) | The method accepts the parameter but ignores it ||
|#

{% note info "" %}

Pass at least one filter: `SEARCH_TEXT` of at least `3` characters, `SEARCH_TYPE`, `SEARCH_TYPES`, `SEARCH_DATE`, `SEARCH_AUTHORS`, or the `SEARCH_DATE_FROM` and `SEARCH_DATE_TO` pair. Otherwise, the method returns the `SEARCH_TEXT_ERROR` error. Multiple filters are combined with the AND condition.

{% endnote %}

### How to Retrieve All Search Results {#pagination}

The method sorts notifications by descending creation date, then by descending `id`.

1. Request the first page without `LAST_ID`. The response contains the `total_results` field — the total number of found notifications
2. For the next page, pass the same filters and `LAST_ID` = `id` of the last element of the `notifications` array. The `total_results` field is absent on these pages
3. Repeat step 2 until the method returns fewer notifications than `LIMIT`

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SEARCH_TEXT":"invoice","SEARCH_TYPE":"rest|rest_notify","SEARCH_DATE_FROM":"2026-11-01T00:00:00+01:00","SEARCH_DATE_TO":"2026-11-10T00:00:00+01:00","LIMIT":20}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.history.search
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SEARCH_TEXT":"invoice","SEARCH_TYPE":"rest|rest_notify","SEARCH_DATE_FROM":"2026-11-01T00:00:00+01:00","SEARCH_DATE_TO":"2026-11-10T00:00:00+01:00","LIMIT":20,"auth":"**put_access_token_here**"}' \
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
    // total_results is returned only on the first page (without LAST_ID)
    type NotifyHistorySearchResult = {
      chat_id: number
      notifications: Array<{
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
      }>
      users: Array<{
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
        idle: false
        last_activity_date: ISODate | false
        mobile_last_date: false
        desktop_last_date: false
        absent: ISODate | false
        departments: number[]
        phones: Record<string, string> | false
        bot_data: object | null
        type: string
        website: string
        email: string
      }>
      total_results?: number
    }

    try {
      const response = await $b24.actions.v2.call.make<NotifyHistorySearchResult>({
        method: 'im.notify.history.search',
        params: {
          SEARCH_TEXT: 'invoice',
          SEARCH_TYPE: 'rest|rest_notify',
          SEARCH_DATE_FROM: '2026-11-01T00:00:00+01:00',
          SEARCH_DATE_TO: '2026-11-10T00:00:00+01:00',
          LIMIT: 20,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Found:', result.total_results, 'On page:', result.notifications.length)

        // Pass the id of the last notification as LAST_ID to get the next page
        const last = result.notifications.at(-1)
        if (last && result.notifications.length === 20) {
          console.info('Next page LAST_ID:', last.id)
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
      async function searchNotifyHistory() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.notify.history.search',
            params: {
              SEARCH_TEXT: 'invoice',
              SEARCH_TYPE: 'rest|rest_notify',
              SEARCH_DATE_FROM: '2026-11-01T00:00:00+01:00',
              SEARCH_DATE_TO: '2026-11-10T00:00:00+01:00',
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
          console.info('Found:', result.total_results, 'On page:', result.notifications.length)

          // Pass the id of the last notification as LAST_ID to get the next page
          const last = result.notifications.at(-1)
          if (last && result.notifications.length === 20) {
            console.info('Next page LAST_ID:', last.id)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchNotifyHistory)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.notify.history.search(
            search_text="invoice",
            search_type="rest|rest_notify",
            search_date_from="2026-11-01T00:00:00+01:00",
            search_date_to="2026-11-10T00:00:00+01:00",
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
            'im.notify.history.search',
            [
                'SEARCH_TEXT' => 'invoice',
                'SEARCH_TYPE' => 'rest|rest_notify',
                'SEARCH_DATE_FROM' => '2026-11-01T00:00:00+01:00',
                'SEARCH_DATE_TO' => '2026-11-10T00:00:00+01:00',
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
        'im.notify.history.search',
        {
            SEARCH_TEXT: 'invoice',
            SEARCH_TYPE: 'rest|rest_notify',
            SEARCH_DATE_FROM: '2026-11-01T00:00:00+01:00',
            SEARCH_DATE_TO: '2026-11-10T00:00:00+01:00',
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
        'im.notify.history.search',
        [
            'SEARCH_TEXT' => 'invoice',
            'SEARCH_TYPE' => 'rest|rest_notify',
            'SEARCH_DATE_FROM' => '2026-11-01T00:00:00+01:00',
            'SEARCH_DATE_TO' => '2026-11-10T00:00:00+01:00',
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
    res, err := client.Core().Call(ctx, "im.notify.history.search", b24.Params{
    	"SEARCH_TEXT":      "invoice",
    	"SEARCH_TYPE":      "rest|rest_notify",
    	"SEARCH_DATE_FROM": "2026-11-01T00:00:00+01:00",
    	"SEARCH_DATE_TO":   "2026-11-10T00:00:00+01:00",
    	"LIMIT":            20,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("im.notify.history.search: %w", err)
    }

    var item struct {
    	ChatID        int `json:"chat_id"`
    	TotalResults  int `json:"total_results"`
    	Notifications []struct {
    		ID   int    `json:"id"`
    		Text string `json:"text"`
    	} `json:"notifications"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.TotalResults, len(item.Notifications))

    // Pass the id of the last notification in LAST_ID for the next page
    if n := len(item.Notifications); n == 20 {
    	fmt.Println("LAST_ID:", item.Notifications[n-1].ID)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "chat_id": 2535,
        "notifications": [
            {
                "id": 44021,
                "chat_id": 2535,
                "author_id": 15,
                "date": "2026-11-10T14:37:49+01:00",
                "notify_type": 2,
                "notify_module": "rest",
                "notify_event": "rest_notify",
                "notify_tag": "",
                "notify_sub_tag": "",
                "notify_title": "",
                "setting_name": "rest|rest_notify",
                "text": "Check invoice [B]#42[/B]",
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
                "last_activity_date": "2026-11-10T14:36:42+01:00",
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
        ],
        "total_results": 1
    },
    "time": {
        "start": 1794317871,
        "finish": 1794317871.341717,
        "duration": 0.341717004776001,
        "processing": 0,
        "date_start": "2026-11-10T14:37:51+01:00",
        "date_finish": "2026-11-10T14:37:51+01:00",
        "operating_reset_at": 1794318420,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the search results [(detailed description)](#result-object).

If the user does not have a system notification chat yet, or only `SEARCH_DATE_FROM` or only `SEARCH_DATE_TO` is passed, the method returns an empty array `[]` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result-object}

#|
|| **Name**
`Type` | **Description** ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the user's system notification chat ||
|| **notifications**
[`array`](../../data-types.md) | Array of found notifications on the page [(detailed description)](#notification-object). If nothing is found, an empty array ||
|| **users**
[`array`](../../data-types.md) | Array of authors of the found notifications [(detailed description)](#user-object). System notifications with `author_id` = `0` are not included in the array ||
|| **total_results**
[`integer`](../../data-types.md) | Total number of found notifications. Present only in the response to a request without `LAST_ID` ||
|#

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
- `4` — system notification ||
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

#### User Object {#user-object}

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
[`boolean`](../../data-types.md) | External user status ||
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
    "error": "SEARCH_TEXT_ERROR",
    "error_description": "SEARCH_TEXT can't be less than 3 symbols"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `SEARCH_TEXT_ERROR` | SEARCH_TEXT can't be less then 3 symbols | No filter is passed, and `SEARCH_TEXT` is not passed or is shorter than `3` characters. The error also occurs if only `GROUP_TAG` is passed ||
|| `400` | `LAST_ID_STRING` | Last notification ID can't be string | The `LAST_ID` value contains characters other than digits, for example `abc` or `-5` ||
|| `400` | `500` | Incorrect date/time: abc | The `SEARCH_DATE`, `SEARCH_DATE_FROM`, or `SEARCH_DATE_TO` value cannot be recognized as a date ||
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
# Get User Data with im.user.list.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.user.list.get` returns data about users based on a list of identifiers.

If the current user is an extranet user, the method will return data only for users from their extranet groups. User identifiers outside these groups will be skipped without error.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`Type` | **Description** ||
|| **ID*** 
[`array`](../../data-types.md) 
[`string`](../../data-types.md) | An array of user identifiers or a JSON string containing an array.

User identifiers can be obtained using the methods [user.get](../../user/user-get.md), [user.search](../../user/user-search.md), or [im.chat.user.list](../chat-users/im-chat-user-list.md) ||
|| **AVATAR_HR**
[`string`](../../data-types.md) | Parameter to request the `avatar_hr` field with the high-resolution avatar URL. Acceptable values: `Y` or `N`, default is `N`.

Currently, the `avatar_hr` field is always returned, regardless of the parameter value ||
|| **RESULT_TYPE**
[`string`](../../data-types.md) | Format of the `result`. The value `array` will return a standard array of user objects, any other value will return an object with user identifier keys ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":[4,5],"AVATAR_HR":"Y","RESULT_TYPE":"array"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.user.list.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":[4,5],"RESULT_TYPE":"array","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.user.list.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each user returned in result[] (match the "response handling" section of the page)
    type ImUserInfo = {
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
      last_activity_date: ISODate
      mobile_last_date: ISODate | false
      desktop_last_date: ISODate | false
      absent: ISODate | false
      departments: number[]
      phones: { work_phone: string; personal_mobile: string; inner_phone: string } | false
      website: string
      email: string
      bot_data: object | null
      type: string
    }

    try {
      const response = await $b24.actions.v2.call.make<ImUserInfo[]>({
        method: 'im.user.list.get',
        params: {
          ID: [4, 5],
          AVATAR_HR: 'Y',
          RESULT_TYPE: 'array',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Users:', result.length, result.map(u => u.name))
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
      async function getImUserList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.user.list.get',
            params: {
              ID: [4, 5],
              AVATAR_HR: 'Y',
              RESULT_TYPE: 'array',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Users:', result.length, result.map(u => u.name))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getImUserList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.user.list.get',
            [
                'ID' => [4, 5],
                'AVATAR_HR' => 'Y',
                'RESULT_TYPE' => 'array',
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
        'im.user.list.get',
        {
            ID: [4, 5],
            AVATAR_HR: 'Y',
            RESULT_TYPE: 'array',
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
        'im.user.list.get',
        [
            'ID' => [4, 5],
            'AVATAR_HR' => 'Y',
            'RESULT_TYPE' => 'array',
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
    "result": [
        {
            "id": 4,
            "active": true,
            "name": "Emily Smith",
            "first_name": "Emily",
            "last_name": "Smith",
            "work_position": "Analyst",
            "color": "#df532d",
            "avatar": "https://example.bitrix24.com/upload/main/avatar4.png",
            "avatar_hr": "https://example.bitrix24.com/upload/main/avatar4_hr.png",
            "gender": "F",
            "birthday": "",
            "extranet": false,
            "network": false,
            "bot": false,
            "connector": false,
            "external_auth_id": "default",
            "status": "online",
            "idle": false,
            "last_activity_date": "2026-03-02T09:30:00+01:00",
            "mobile_last_date": false,
            "desktop_last_date": false,
            "absent": false,
            "departments": [7, 5],
            "phones": {
                "work_phone": "+12123456789",
                "personal_mobile": "+12123456789",
                "inner_phone": "22"
            },
            "website": "example.com",
            "email": "emily.smith@example.com",
            "bot_data": null,
            "type": "user"
        },
        {
            "id": 5,
            "active": true,
            "name": "John Johnson",
            "first_name": "John",
            "last_name": "Johnson",
            "work_position": "Manager",
            "color": "#048bd0",
            "avatar": "https://example.bitrix24.com/upload/main/avatar.png",
            "avatar_hr": "https://example.bitrix24.com/upload/main/avatar_hr.png",
            "gender": "M",
            "birthday": "",
            "extranet": false,
            "network": false,
            "bot": false,
            "connector": false,
            "external_auth_id": "default",
            "status": "online",
            "idle": false,
            "last_activity_date": "2026-03-02T09:30:00+01:00",
            "mobile_last_date": false,
            "desktop_last_date": false,
            "absent": false,
            "departments": [10],
            "phones": {
                "work_phone": "+12123456788",
                "personal_mobile": "+12123456788",
                "inner_phone": "21"
            },
            "website": "example.com",
            "email": "user@example.com",
            "bot_data": null,
            "type": "user"
        }
    ],
    "time": {
        "start": 1772449081,
        "finish": 1772449081.887056,
        "duration": 0.8870561122894287,
        "processing": 0,
        "date_start": "2026-03-02T13:58:01+01:00",
        "date_finish": "2026-03-02T13:58:01+01:00",
        "operating_reset_at": 1772449681,
        "operating": 0
    }
}
```

## Returned Data

#| 
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) 
[`array`](../../data-types.md) | User data. By default, an object with identifier keys is returned; when `RESULT_TYPE = 'array'`, an array is returned.

The structure of the user object is described in detail [below](#user-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### User Object {#user-object}

#| 
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | User identifier ||
|| **active**
[`boolean`](../../data-types.md) | User activity status ||
|| **name**
[`string`](../../data-types.md) | User's full name ||
|| **first_name**
[`string`](../../data-types.md) | User's first name ||
|| **last_name**
[`string`](../../data-types.md) | User's last name ||
|| **work_position**
[`string`](../../data-types.md) | User's job title ||
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
[`boolean`](../../data-types.md) | Extranet user status ||
|| **network**
[`boolean`](../../data-types.md) | Bitrix24 Network user status ||
|| **bot**
[`boolean`](../../data-types.md) | Bot status ||
|| **connector**
[`boolean`](../../data-types.md) | Open Channels user status ||
|| **external_auth_id**
[`string`](../../data-types.md) | External authorization code ||
|| **status**
[`string`](../../data-types.md) | User status ||
|| **idle**
[`datetime`](../../data-types.md) | User's idle date or `false` ||
|| **last_activity_date**
[`datetime`](../../data-types.md) | User's last activity date ||
|| **mobile_last_date**
[`datetime`](../../data-types.md) | User's last activity date in the mobile app or `false` ||
|| **desktop_last_date**
[`datetime`](../../data-types.md) | User's last activity date in the desktop app or `false` ||
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
    "error": "INVALID_FORMAT",
    "error_description": "A wrong format for the ID field is passed"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `INVALID_FORMAT` | A wrong format for the ID field is passed | The `ID` parameter is not provided or is in an incorrect format ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-user-get.md)
- [{#T}](./im-user-status-set.md)
- [{#T}](./im-user-status-get.md)
- [{#T}](./im-user-status-idle-start.md)
- [{#T}](./im-user-status-idle-end.md)
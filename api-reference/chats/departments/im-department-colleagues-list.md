# Get the List of Colleagues for the Current User im.department.colleagues.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any intranet user, except bots

The method `im.department.colleagues.list` retrieves the list of colleagues for the current user. For a manager, the method will return a list of subordinates and all supervisors.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **USER_DATA**
[`string`](../../data-types.md) | Return detailed user data. 

Possible values:
- `Y` — yes
- `N` — no ||
|| **OFFSET**
[`integer`](../../data-types.md) | Offset for user selection ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of items in the selection. Default is `10`. Maximum value is `50` ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_DATA":"Y","OFFSET":0,"LIMIT":5}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.department.colleagues.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"USER_DATA":"Y","OFFSET":0,"LIMIT":5,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.department.colleagues.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each ColleagueUser returned in result[]
    type ColleagueUser = {
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
      phones: { personal_mobile?: string; inner_phone?: string } | false
      bot_data: object | null
      type: string
      website: string
      email: string
    }

    try {
      // im.department.colleagues.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ColleagueUser[]>({
        method: 'im.department.colleagues.list',
        params: {
          USER_DATA: 'Y',
          OFFSET: 0,
          LIMIT: 5,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Colleagues:', result.length, result)
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
      async function fetchColleagueList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // im.department.colleagues.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'im.department.colleagues.list',
            params: {
              USER_DATA: 'Y',
              OFFSET: 0,
              LIMIT: 5,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Colleagues:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchColleagueList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.department.colleagues.list',
                [
                    'USER_DATA' => 'Y',
                    'OFFSET' => 0,
                    'LIMIT' => 5,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.department.colleagues.list',
        {
            USER_DATA: 'Y',
            OFFSET: 0,
            LIMIT: 5,
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
                console.log(result.total());
                console.log(result.answer.next);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.department.colleagues.list',
        [
            'USER_DATA' => 'Y',
            'OFFSET' => 0,
            'LIMIT' => 5,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

- When `USER_DATA = 'N'`:

    ```json
    {
        "result": [9,547,408,103,290],
        "next": 5,
        "total": 7,
        "time": {
            "start": 1772520802,
            "finish": 1772520802.36194,
            "duration": 0.3619399070739746,
            "processing": 0,
            "date_start": "2026-03-03T09:53:22+01:00",
            "date_finish": "2026-03-03T09:53:22+01:00",
            "operating_reset_at": 1772521402,
            "operating": 0
        }
    }
    ```

- When `USER_DATA = 'Y'`:

    ```json
    {
        "result": [
            {
                "id": 9,
                "active": true,
                "name": "Anna Smith",
                "first_name": "Anna",
                "last_name": "Smith",
                "work_position": "Project Manager",
                "color": "#58cc47",
                "avatar": "https://mysite.com/upload/avatars/anna-smith.jpg",
                "avatar_hr": "https://mysite.com/upload/avatars/anna-smith.jpg",
                "gender": "F",
                "birthday": "",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "external_auth_id": "socservices",
                "status": "online",
                "idle": false,
                "last_activity_date": "2023-03-10T17:16:44+01:00",
                "mobile_last_date": false,
                "desktop_last_date": false,
                "absent": false,
                "departments": [
                    1,
                    667
                ],
                "phones": false,
                "bot_data": null,
                "type": "user",
                "website": "",
                "email": "anna.smith@mysite.com"
            },
            ... // description for each user
        ],
        "next": 5,
        "total": 7,
        "time": {
            "start": 1772521273,
            "finish": 1772521273.83899,
            "duration": 0.8389899730682373,
            "processing": 0,
            "date_start": "2026-03-03T10:01:13+01:00",
            "date_finish": "2026-03-03T10:01:13+01:00",
            "operating_reset_at": 1772521873,
            "operating": 0
        }
    }
    ```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of users. 
- When `USER_DATA = 'N'`, contains user IDs
- When `USER_DATA = 'Y'`, contains user objects [(detailed description)](#user-object) ||
|| **total**
[`integer`](../../data-types.md) | Total number of users in the selection ||
|| **next**
[`integer`](../../data-types.md) | Offset for retrieving the next page. This field is returned if there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### User Object {#user-object}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | User ID ||
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
[`string`](../../data-types.md) | Link to avatar ||
|| **avatar_hr**
[`string`](../../data-types.md) | Link to high-resolution avatar ||
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
[`datetime`](../../data-types.md) | Last activity date in the mobile app or `false` ||
|| **desktop_last_date**
[`datetime`](../../data-types.md) | Last activity date in the desktop app or `false` ||
|| **absent**
[`datetime`](../../data-types.md) | User's absence end date or `false` ||
|| **departments**
[`array`](../../data-types.md) | Array of department IDs ||
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
`type` | **Description** ||
|| **personal_mobile**
[`string`](../../data-types.md) | Mobile phone ||
|| **inner_phone**
[`string`](../../data-types.md) | Internal phone ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "ACCESS_ERROR",
    "error_description": "Only intranet users have access to this method."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_ERROR` | Only intranet users have access to this method | The method is not available for extranet users and bots ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-department-get.md)
- [{#T}](./im-department-managers-get.md)
- [{#T}](./im-department-employees-get.md)
- [{#T}](./im-department-colleagues-list.md)
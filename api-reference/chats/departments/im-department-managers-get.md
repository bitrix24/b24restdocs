# Get a List of Department Managers im.department.managers.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any intranet user, except bots

The method `im.department.managers.get` retrieves a list of managers for the specified departments.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`array`](../../data-types.md) | An array of department identifiers. You can pass a string with a JSON array of identifiers.

You can obtain the department ID using the [get department list](../../departments/department-get.md) or the [search departments by name](../search/im-search-department-list.md) ||
|| **USER_DATA**
[`string`](../../data-types.md) | Return detailed user data.  

Possible values:
- `Y` — yes
- `N` — no
||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":[3,7],"USER_DATA":"Y"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.department.managers.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":[3,7],"USER_DATA":"Y","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.department.managers.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ManagerUser = {
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
      phones: { personal_mobile: string; inner_phone: string } | false
      bot_data: Record<string, unknown> | null
      type: string
      website: string
      email: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DepartmentManagersResult = Record<string, ManagerUser[]>

    try {
      const response = await $b24.actions.v2.call.make<DepartmentManagersResult>({
        method: 'im.department.managers.get',
        params: {
          ID: [3, 7],
          USER_DATA: 'Y',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        for (const [deptId, managers] of Object.entries(result)) {
          console.info(`Department ${deptId}:`, managers.map(m => `${m.name} (id: ${m.id})`))
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
      async function getDepartmentManagers() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.department.managers.get',
            params: {
              ID: [3, 7],
              USER_DATA: 'Y',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          for (const [deptId, managers] of Object.entries(result)) {
            console.info(`Department ${deptId}:`, managers.map(m => `${m.name} (id: ${m.id})`))
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDepartmentManagers)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.department.managers.get',
                [
                    'ID' => [3, 7],
                    'USER_DATA' => 'Y',
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
        'im.department.managers.get',
        {
            ID: [3, 7],
            USER_DATA: 'Y'
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
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.department.managers.get',
        [
            'ID' => [3, 7],
            'USER_DATA' => 'Y',
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
        "result": {
            "3": [13],
            "7": [103]
        },
        "time": {
            "start": 1772519172,
            "finish": 1772519172.208348,
            "duration": 0.20834803581237793,
            "processing": 0,
            "date_start": "2026-03-03T09:26:12+01:00",
            "date_finish": "2026-03-03T09:26:12+01:00",
            "operating_reset_at": 1772519772,
            "operating": 0
        }
    }
    ```

- When `USER_DATA = 'Y'`:

    ```json
    {
        "result": {
            "3": [
                {
                    "id": 13,
                    "active": true,
                    "name": "John Smith",
                    "first_name": "John",
                    "last_name": "Smith",
                    "work_position": "Chief Accountant",
                    "color": "#728f7a",
                    "avatar": "https://mysite.com/upload/avatars/john-smith.jpg",
                    "avatar_hr": "https://mysite.com/upload/avatars/john-smith.jpg",
                    "gender": "M",
                    "birthday": "10-09",
                    "extranet": false,
                    "network": false,
                    "bot": false,
                    "connector": false,
                    "external_auth_id": "socservices",
                    "status": "online",
                    "idle": false,
                    "last_activity_date": "2024-02-19T00:40:41+01:00",
                    "mobile_last_date": false,
                    "desktop_last_date": false,
                    "absent": false,
                    "departments": [
                        9,
                        3
                    ],
                    "phones": {
                        "personal_mobile": "12134567890",
                        "inner_phone": "55"
                    },
                    "bot_data": null,
                    "type": "user",
                    "website": "",
                    "email": "john.smith@mysite.com"
                }
            ],
            "7": [
                {
                    "id": 103,
                    "active": true,
                    "name": "Anna Johnson",
                    "first_name": "Anna",
                    "last_name": "Johnson",
                    "work_position": "Head of Development Department",
                    "color": "#4ba984",
                    "avatar": "https://mysite.com/upload/avatars/anna-johnson.jpg",
                    "avatar_hr": "https://mysite.com/upload/avatars/anna-johnson.jpg",
                    "gender": "F",
                    "birthday": "",
                    "extranet": false,
                    "network": false,
                    "bot": false,
                    "connector": false,
                    "external_auth_id": "socservices",
                    "status": "online",
                    "idle": false,
                    "last_activity_date": "2025-11-06T16:59:28+01:00",
                    "mobile_last_date": false,
                    "desktop_last_date": false,
                    "absent": false,
                    "departments": [
                        1,
                        7
                    ],
                    "phones": false,
                    "bot_data": null,
                    "type": "user",
                    "website": "",
                    "email": "anna.johnson@mysite.com"
                }
            ]
        },
        "time": {
            "start": 1772519165,
            "finish": 1772519165.292582,
            "duration": 0.29258203506469727,
            "processing": 0,
            "date_start": "2026-03-03T09:26:05+01:00",
            "date_finish": "2026-03-03T09:26:05+01:00",
            "operating_reset_at": 1772519765,
            "operating": 0
        }
    }
    ```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root object where the key is the department identifier, and the value:
- when `USER_DATA = 'N'` contains an array of manager identifiers,
- when `USER_DATA = 'Y'` contains an array of objects describing the users — department managers [(detailed description)](#user-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### User Object {#user-object}

#|
|| **Name**
`type` | **Description** ||
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
`type` | **Description** ||
|| **personal_mobile**
[`string`](../../data-types.md) | Mobile phone ||
|| **inner_phone**
[`string`](../../data-types.md) | Internal phone ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ID_EMPTY",
    "error_description": "Department ID can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ID_EMPTY` | Department ID can't be empty | The required parameter `ID` is missing, incorrectly provided, or empty ||
|| `403` | `ACCESS_ERROR` | Only intranet users have access to this method | The method is not available for extranet users and bots ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-department-get.md)
- [{#T}](./im-department-managers-get.md)
- [{#T}](./im-department-employees-get.md)
- [{#T}](./im-department-colleagues-list.md)
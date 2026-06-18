# Find Departments im.search.department.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.department.list` performs a search for departments by their full name.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **FIND***
[`string`](../../data-types.md) | Search phrase for finding the full name of the department (field [full_name](../departments/im-department-get.md#department)) ||
|| **USER_DATA**
[`string`](../../data-types.md) | Return the manager's data in the field [manager_user_data](#manager_user_data). 

Available values: 
- `Y` — yes
- `N` — no

Default value — `N` ||
|| **OFFSET**
[`integer`](../../data-types.md) | Offset for the department selection. Default is `0` ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of items in the selection. Default is `10`. Maximum value is `50` ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FIND":"Department","USER_DATA":"Y","OFFSET":0,"LIMIT":10}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.search.department.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FIND":"Department","USER_DATA":"Y","OFFSET":0,"LIMIT":10,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.search.department.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ManagerUserData = {
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
      phones: { work_phone?: string; inner_phone?: string } | false
      bot_data: object | null
      type: string
      website: string
      email: string
    }

    // Shape of each department item returned in result[]
    type DepartmentItem = {
      id: number
      name: string
      full_name: string
      manager_user_id: number
      manager_user_data?: ManagerUserData
    }

    try {
      // im.search.department.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<DepartmentItem[]>({
        method: 'im.search.department.list',
        params: {
          FIND: 'Department',
          USER_DATA: 'Y',
          OFFSET: 0,
          LIMIT: 10,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Found departments:', result.length, result)
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
      async function searchDepartmentList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // im.search.department.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'im.search.department.list',
            params: {
              FIND: 'Department',
              USER_DATA: 'Y',
              OFFSET: 0,
              LIMIT: 10,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Found departments:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchDepartmentList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.search.department.list',
            [
                'FIND' => 'Department',
                'USER_DATA' => 'Y',
                'OFFSET' => 0,
                'LIMIT' => 10,
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
        'im.search.department.list',
        {
            FIND: 'Department',
            USER_DATA: 'Y',
            OFFSET: 0,
            LIMIT: 10,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data(), result.total(), result.next());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.search.department.list',
        [
            'FIND' => 'Department',
            'USER_DATA' => 'Y',
            'OFFSET' => 0,
            'LIMIT' => 10,
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
            "id": 9,
            "name": "Marketing and Advertising Department",
            "full_name": "Marketing and Advertising Department / My Company",
            "manager_user_id": 3,
            "manager_user_data": {
                "id": 3,
                "active": true,
                "name": "Catherine Peterson",
                "first_name": "Catherine",
                "last_name": "Peterson",
                "work_position": "",
                "color": "#1eb4aa",
                "avatar": "https://example.bitrix24.com/upload/main/avatar.png",
                "avatar_hr": "https://example.bitrix24.com/upload/main/avatar.png",
                "gender": "F",
                "birthday": "06-04",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "external_auth_id": "socservices",
                "status": "online",
                "idle": false,
                "last_activity_date": "2026-03-04T22:08:29+01:00",
                "mobile_last_date": false,
                "desktop_last_date": false,
                "absent": false,
                "departments": [1],
                "phones": {
                    "work_phone": "1415551111",
                    "inner_phone": "222"
                },
                "bot_data": null,
                "type": "user",
                "website": "example.com",
                "email": "user@example.com"
            }
        },
        ... // description for each department
    ],
    "total": 2,
    "time": {
        "start": 1772651443,
        "finish": 1772651443.378436,
        "duration": 0.3784360885620117,
        "processing": 0,
        "date_start": "2026-03-04T22:10:43+01:00",
        "date_finish": "2026-03-04T22:10:43+01:00",
        "operating_reset_at": 1772652043,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of found departments.

The structure of the department object is described in detail [below](#department-object) ||
|| **total**
[`integer`](../../data-types.md) | Total number of found departments ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. This field is returned if there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

### Department Object {#department-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Department identifier ||
|| **name**
[`string`](../../data-types.md) | Short name of the department ||
|| **full_name**
[`string`](../../data-types.md) | Full name of the department ||
|| **manager_user_id**
[`integer`](../../data-types.md) | Identifier of the department manager ||
|| **manager_user_data**
[`object`](../../data-types.md) | Data of the department manager. The object is returned only when `USER_DATA = 'Y'`.

The structure of the manager object is described in detail [below](#manager_user_data) ||
|#

#### Manager User Data Object {#manager_user_data}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | User identifier ||
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
[`string`](../../data-types.md) | Link to avatar ||
|| **avatar_hr**
[`string`](../../data-types.md) | Link to high-resolution avatar ||
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
    "error": "FIND_SHORT",
    "error_description": "Too short a search phrase."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `FIND_SHORT` | Too short a search phrase | The `FIND` parameter is not provided or the phrase is less than three characters ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-search-chat-list.md)
- [{#T}](./im-search-user-list.md)
- [{#T}](./im-search-last-add.md)
- [{#T}](./im-search-last-get.md)
- [{#T}](./im-search-last-delete.md)
# Find Users im.search.user.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.user.list` allows you to search for users by first name, last name, job title, and department.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **FIND***
[`string`](../../data-types.md) | Search phrase. The minimum number of characters for the search is `3` ||
|| **BUSINESS**
[`string`](../../data-types.md) | Search only among business users. 

Allowed values:
- `Y` — yes
- `N` — no

Default value is `N` ||

|| **OFFSET**
[`integer`](../../data-types.md) | Offset for the user selection. Default is `0` ||
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
      -d '{"FIND":"John","BUSINESS":"N","OFFSET":0,"LIMIT":10}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.search.user.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FIND":"John","BUSINESS":"N","OFFSET":0,"LIMIT":10,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.search.user.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each user returned in result[]
    type UserResult = {
      id: number
      name: string
      first_name: string
      last_name: string
      work_position: string
      color: string
      avatar: string | null
      gender: string
      birthday: string | false
      extranet: boolean
      network: boolean
      bot: boolean
      connector: boolean
      external_auth_id: string
      status: string
      idle: ISODate | false
      last_activity_date: ISODate | false
      mobile_last_date: ISODate | false
      departments: number[]
      absent: ISODate | false
      phones: {
        work_phone: string
        personal_mobile: string
        inner_phone: string
      } | false
    }

    try {
      // im.search.user.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<UserResult[]>({
        method: 'im.search.user.list',
        params: {
          FIND: 'Ivan',
          BUSINESS: 'N',
          OFFSET: 0,
          LIMIT: 10,
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Found users:', result.length, result[0]?.name)
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
      async function searchUserList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // im.search.user.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'im.search.user.list',
            params: {
              FIND: 'Ivan',
              BUSINESS: 'N',
              OFFSET: 0,
              LIMIT: 10,
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Found users:', result.length, result[0]?.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchUserList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.search.user.list',
            [
                'FIND' => 'John',
                'BUSINESS' => 'N',
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
        'im.search.user.list',
        {
            FIND: 'John',
            BUSINESS: 'N',
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
        'im.search.user.list',
        [
            'FIND' => 'John',
            'BUSINESS' => 'N',
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

HTTP Status Code: **200**

```json
{
    "result": [
        {
            "id": 103,
            "name": "Emily Smith",
            "first_name": "Emily",
            "last_name": "Smith",
            "work_position": "IT Department Head",
            "color": "#4ba984",
            "avatar": "https://example.bitrix24.com/upload/main/avatar.png",
            "gender": "F",
            "birthday": "08-03",
            "extranet": false,
            "network": false,
            "bot": false,
            "connector": false,
            "external_auth_id": "socservices",
            "status": "online",
            "idle": false,
            "last_activity_date": "2026-03-04T15:40:56+01:00",
            "mobile_last_date": false,
            "departments": [1, 7],
            "absent": false,
            "phones": {
                "work_phone": "19123456789",
                "personal_mobile": "12123456789",
                "inner_phone": "78"
            }
        },
        ... // description for each user
    ],
    "total": 2,
    "time": {
        "start": 1772628089,
        "finish": 1772628089.061656,
        "duration": 0.06165599822998047,
        "processing": 0,
        "date_start": "2026-03-04T15:41:29+01:00",
        "date_finish": "2026-03-04T15:41:29+01:00",
        "operating_reset_at": 1772628689,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of found users.

The structure of the user object is described in detail [below](#user-object) ||
|| **total**
[`integer`](../../data-types.md) | Total number of found users ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. This field is returned if there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### User Object {#user-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | User identifier ||
|| **name**
[`string`](../../data-types.md) | User's full name ||
|| **first_name**
[`string`](../../data-types.md) | User's first name ||
|| **last_name**
[`string`](../../data-types.md) | User's last name ||
|| **work_position**
[`string`](../../data-types.md) | User's job title ||
|| **color**
[`string`](../../data-types.md) | User's color in HEX format ||
|| **avatar**
[`string`](../../data-types.md) 
[`null`](../../data-types.md) | Link to the user's avatar ||
|| **gender**
[`string`](../../data-types.md) | User's gender: `M` or `F` ||
|| **birthday**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Birthday in `DD-MM` format or `false` ||
|| **extranet**
[`boolean`](../../data-types.md) | Indicator of extranet user ||
|| **network**
[`boolean`](../../data-types.md) | Indicator of Bitrix24 Network user ||
|| **bot**
[`boolean`](../../data-types.md) | Indicator of bot user ||
|| **connector**
[`boolean`](../../data-types.md) | Indicator of open channels connector user ||
|| **external_auth_id**
[`string`](../../data-types.md) | External authorization identifier ||
|| **status**
[`string`](../../data-types.md) | Current status of the user ||
|| **idle**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Time of transition to "Away" status in ISO 8601 (RFC3339) format or `false` ||
|| **last_activity_date**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Time of last activity in ISO 8601 (RFC3339) format or `false` ||
|| **mobile_last_date**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Time of last mobile activity in ISO 8601 (RFC3339) format or `false` ||
|| **departments**
[`array`](../../data-types.md) | Array of department identifiers ||
|| **absent**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Date of absence end in ISO 8601 (RFC3339) format or `false` ||
|| **phones**
[`object`](../../data-types.md) | User's phones or `false`.

The structure of the object is described in detail [below](#phones-object) ||
|#

### Phones Object {#phones-object}

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
    "error": "FIND_SHORT",
    "error_description": "Too short a search phrase."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `FIND_SHORT` | Too short a search phrase | The search phrase is not provided or is too short for the internal search filter ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-search-chat-list.md)
- [{#T}](./im-search-department-list.md)
- [{#T}](./im-search-last-add.md)
- [{#T}](./im-search-last-get.md)
- [{#T}](./im-search-last-delete.md)
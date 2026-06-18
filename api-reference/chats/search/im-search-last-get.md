# Get Search History im.search.last.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.last.get` returns a list of dialogs from the history of the last search.

This method was designed for the previous version of the chat. In the current M1 chat version, it works, but the results are not displayed in the interface.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **SKIP_OPENLINES**
[`string`](../../data-types.md) | Skip Open Channels chats.

Possible values:
- `Y` — yes
- `N` — no 

Default value — `N` ||
|| **SKIP_CHAT**
[`string`](../../data-types.md) | Skip group chats.

Possible values:
- `Y` — yes
- `N` — no 

Default value — `N` ||
|| **SKIP_DIALOG**
[`string`](../../data-types.md) | Skip personal dialogs. 

Possible values:
- `Y` — yes
- `N` — no 

Default value — `N` ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SKIP_OPENLINES":"N","SKIP_CHAT":"N","SKIP_DIALOG":"N"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.search.last.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"SKIP_OPENLINES":"N","SKIP_CHAT":"N","SKIP_DIALOG":"N","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.search.last.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each SearchLastItem returned in result[]
    type SearchLastItem = {
      id: string | number
      type: 'chat' | 'user'
      avatar: {
        url: string
        color: string
      }
      title: string
      chat?: {
        id: number
        name: string
        owner: number
        extranet: boolean
        avatar: string
        color: string
        type: string
        entity_type: string
        entity_id: string
        entity_data_1: string
        entity_data_2: string
        entity_data_3: string
        mute_list: unknown[]
        date_create: ISODate
        message_type: string
      }
      user?: {
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
        phones: { personal_mobile: string; work_phone: string; inner_phone: string } | false
        bot_data: object | null
        type: string
        website: string
        email: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<SearchLastItem[]>({
        method: 'im.search.last.get',
        params: {
          SKIP_OPENLINES: 'N',
          SKIP_CHAT: 'N',
          SKIP_DIALOG: 'N',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Search history entries:', result.length, result)
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
      async function getSearchHistory() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.search.last.get',
            params: {
              SKIP_OPENLINES: 'N',
              SKIP_CHAT: 'N',
              SKIP_DIALOG: 'N',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Search history entries:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSearchHistory)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.search.last.get',
            [
                'SKIP_OPENLINES' => 'N',
                'SKIP_CHAT' => 'N',
                'SKIP_DIALOG' => 'N',
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
        'im.search.last.get',
        {
            SKIP_OPENLINES: 'N',
            SKIP_CHAT: 'N',
            SKIP_DIALOG: 'N',
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
        'im.search.last.get',
        [
            'SKIP_OPENLINES' => 'N',
            'SKIP_CHAT' => 'N',
            'SKIP_DIALOG' => 'N',
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
            "id": "chat1157",
            "type": "chat",
            "avatar": {
                "url": "",
                "color": "#ab7761"
            },
            "title": "Brown Chat #18",
            "chat": {
                "id": 1157,
                "name": "Brown Chat #18",
                "owner": 27,
                "extranet": false,
                "avatar": "",
                "color": "#ab7761",
                "type": "thread",
                "entity_type": "THREAD",
                "entity_id": "",
                "entity_data_1": "",
                "entity_data_2": "",
                "entity_data_3": "",
                "mute_list": [],
                "date_create": "2025-01-30T00:41:03+01:00",
                "message_type": "C"
            }
        },
        {
            "id": 103,
            "type": "user",
            "avatar": {
                "url": "https://example.bitrix24.com/upload/main/avatar.png",
                "color": "#4ba984"
            },
            "title": "Emily Smith",
            "user": {
                "id": 103,
                "active": true,
                "name": "Emily Smith",
                "first_name": "Emily",
                "last_name": "Smith",
                "work_position": "IT Department Head",
                "color": "#4ba984",
                "avatar": "https://example.bitrix24.com/upload/main/avatar.png",
                "avatar_hr": "https://example.bitrix24.com/upload/main/avatar.png",
                "gender": "F",
                "birthday": "08-03",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "external_auth_id": "socservices",
                "status": "online",
                "idle": false,
                "last_activity_date": "2026-03-05T10:19:37+01:00",
                "mobile_last_date": false,
                "desktop_last_date": false,
                "absent": false,
                "departments": [1, 7],
                "phones": {
                    "personal_mobile": "81234567890",
                    "work_phone": "19123456789",
                    "inner_phone": "78"
                },
                "bot_data": null,
                "type": "user",
                "website": "",
                "email": "emily@example.com"
            }
        },
        ... // description for each chat, user
    ],
    "time": {
        "start": 1772695649,
        "finish": 1772695649.89509,
        "duration": 0.8950901031494141,
        "processing": 0,
        "date_start": "2026-03-05T10:27:29+01:00",
        "date_finish": "2026-03-05T10:27:29+01:00",
        "operating_reset_at": 1772696249,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of search history items.

The structure of the item object is described in detail [below](#last-item-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Search History Item {#last-item-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`string`](../../data-types.md) 
[`integer`](../../data-types.md) | Identifier of the chat or user identifier for personal dialog ||
|| **type**
[`string`](../../data-types.md) | Record type: `chat` or `user` ||
|| **avatar**
[`object`](../../data-types.md) | Avatar data of the record.

The structure of the object is described in detail [below](#avatar-object) ||
|| **title**
[`string`](../../data-types.md) | Title of the record ||
|| **user**
[`object`](../../data-types.md) | User data for `user` type record.

The structure of the object is described in detail [below](#user-object) ||
|| **chat**
[`object`](../../data-types.md) | Chat data for `chat` type record.

The structure of the object is described in detail [below](#chat-object) ||
|#

### Avatar Object {#avatar-object}

#|
|| **Name**
`Type` | **Description** ||
|| **url**
[`string`](../../data-types.md) | Link to the avatar ||
|| **color**
[`string`](../../data-types.md) | Color in HEX format ||
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
[`datetime`](../../data-types.md) | User idle date or `false` ||
|| **last_activity_date**
[`datetime`](../../data-types.md) | User's last activity date ||
|| **mobile_last_date**
[`datetime`](../../data-types.md) | Last activity date in the mobile app or `false` ||
|| **desktop_last_date**
[`datetime`](../../data-types.md) | Last activity date in the desktop app or `false` ||
|| **absent**
[`datetime`](../../data-types.md) | Date of the user's absence end or `false` ||
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

### Chat Object {#chat-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Chat identifier ||
|| **name**
[`string`](../../data-types.md) | Chat name ||
|| **owner**
[`integer`](../../data-types.md) | Chat owner identifier ||
|| **extranet**
[`boolean`](../../data-types.md) | Extranet user participation status in the chat ||
|| **avatar**
[`string`](../../data-types.md) 
[`null`](../../data-types.md) | Link to the chat avatar ||
|| **color**
[`string`](../../data-types.md) | Chat color in HEX format ||
|| **type**
[`string`](../../data-types.md) | Chat type ||
|| **entity_type**
[`string`](../../data-types.md) | Type of the object to which the chat is linked ||
|| **entity_id**
[`string`](../../data-types.md) | Identifier of the object to which the chat is linked ||
|| **entity_data_1**
[`string`](../../data-types.md) | Additional data of the chat object — field 1 ||
|| **entity_data_2**
[`string`](../../data-types.md) | Additional data of the chat object — field 2 ||
|| **entity_data_3**
[`string`](../../data-types.md) | Additional data of the chat object — field 3 ||
|| **mute_list**
[`object`](../../data-types.md) | List of users with notifications turned off ||
|| **date_create**
[`string`](../../data-types.md) | Chat creation date in ISO 8601 format (RFC3339) ||
|| **message_type**
[`string`](../../data-types.md) | Message type ||
|#

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-search-chat-list.md)
- [{#T}](./im-search-department-list.md)
- [{#T}](./im-search-user-list.md)
- [{#T}](./im-search-last-add.md)
- [{#T}](./im-search-last-delete.md)
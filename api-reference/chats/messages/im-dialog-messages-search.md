# Find Messages in Chat im.dialog.messages.search

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: a chat participant or a user with access to an open chat

The method `im.dialog.messages.search` searches for messages in a chat by text and date.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat to search for messages in — a number without the `chat` prefix. You can retrieve it with the [im.chat.get](../im-chat-get.md) method ||
|| **SEARCH_MESSAGE**
[`string`](../../data-types.md) | Text to find in messages.

The method searches for text matches only if the search string is longer than 2 characters. If you pass 1–2 characters, the method returns chat messages without filtering by text ||
|| **DATE_FROM**
[`datetime`](../../data-types.md) | Start of the search period in ISO 8601 format (RFC3339) ||
|| **DATE_TO**
[`datetime`](../../data-types.md) | End of the search period in ISO 8601 format (RFC3339) ||
|| **DATE**
[`datetime`](../../data-types.md) | Date in ISO 8601 format (RFC3339) for which messages are needed.

If the parameter is provided, the search is performed within 24 hours from the specified date ||
|| **ORDER**
[`object`](../../data-types.md) | Sorting. One field is available:
- `ID` — sort direction, values `ASC` (ascending) or `DESC` (descending). The method sorts by message creation date, then by `id`

Default — `{"ID": "DESC"}` ||
|| **LIMIT**
[`integer`](../../data-types.md) | How many messages to return.

Default — `50`. The maximum value is `200`: the method reduces a larger value to `200` and replaces `0` and negative values with `50` ||
|| **LAST_ID**
[`integer`](../../data-types.md) | Identifier of the last message on the previous page — the `id` field of the last element of the `messages` array. The method returns messages that follow it in the `ORDER` order.

Do not pass it for the first page ||
|#

### How to Get All Search Results {#pagination}

Request the first page without `LAST_ID`. For the next page, pass the `id` value of the last element of the `messages` array to `LAST_ID`: with `DESC` sorting, the method returns messages with a smaller `id`; with `ASC` sorting, it returns messages with a larger `id`. You have retrieved all results when a page contains fewer messages than the page size, which is the `LIMIT` value subject to the restrictions in the parameter table.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":3,"SEARCH_MESSAGE":"test","ORDER":{"ID":"DESC"},"LIMIT":20}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.dialog.messages.search
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":3,"SEARCH_MESSAGE":"test","ORDER":{"ID":"DESC"},"LIMIT":20,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.dialog.messages.search
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DialogMessagesSearchResult = {
      messages: Array<{
        id: number
        chatId: number
        chat_id: number
        authorId: number
        author_id: number
        date: ISODate
        text: string
        isSystem: boolean
        uuid: string | null
        forward: object | null
        params: Record<string, unknown> | unknown[]
        viewedByOthers: boolean
        unread: boolean
        viewed: boolean
        block: object | null
      }>
      users: unknown[]
      files: unknown[]
      additionalMessages: unknown[]
      copilot: object | null
      stickers: unknown[]
      reactions: unknown[]
      tariffRestrictions: { isHistoryLimitExceeded: boolean }
      usersShort: unknown[]
    }

    try {
      const response = await $b24.actions.v2.call.make<DialogMessagesSearchResult>({
        method: 'im.dialog.messages.search',
        params: {
          CHAT_ID: 3,
          SEARCH_MESSAGE: 'test',
          ORDER: { ID: 'DESC' },
          LIMIT: 20,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Found messages:', result.messages.length, result.messages)
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
      async function searchDialogMessages() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.dialog.messages.search',
            params: {
              CHAT_ID: 3,
              SEARCH_MESSAGE: 'test',
              ORDER: { ID: 'DESC' },
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
          console.info('Found messages:', result.messages.length, result.messages)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchDialogMessages)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.dialog.messages.search(
            chat_id=3,
            search_message="test",
            order={
                "ID": "DESC",
            },
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
        $response = $b24Service
            ->core
            ->call(
                'im.dialog.messages.search',
                [
                    'CHAT_ID' => 3,
                    'SEARCH_MESSAGE' => 'test',
                    'ORDER' => ['ID' => 'DESC'],
                    'LIMIT' => 20,
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
        'im.dialog.messages.search',
        {
            CHAT_ID: 3,
            SEARCH_MESSAGE: 'test',
            ORDER: { ID: 'DESC' },
            LIMIT: 20
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
        'im.dialog.messages.search',
        [
            'CHAT_ID' => 3,
            'SEARCH_MESSAGE' => 'test',
            'ORDER' => ['ID' => 'DESC'],
            'LIMIT' => 20,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.dialog.messages.search", b24.Params{
    	"CHAT_ID":        3,
    	"SEARCH_MESSAGE": "test",
    	"ORDER": b24.Params{
    		"ID": "DESC",
    	},
    	"LIMIT": 20,
    })
    if err != nil {
    	return fmt.Errorf("im.dialog.messages.search: %w", err)
    }

    // The response shape is shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "users": [
            {
                "id": 1,
                "active": true,
                "name": "Alex",
                "firstName": "Alex",
                "lastName": "",
                "workPosition": "",
                "color": "#df532d",
                "avatar": "https://cdn-com.bitrix24.com/path/avatar.jpg",
                "avatarHr": "https://cdn-com.bitrix24.com/path/avatar.jpg",
                "gender": "F",
                "birthday": "",
                "extranet": false,
                "network": false,
                "bot": false,
                "connector": false,
                "externalAuthId": "socservices",
                "internalAccount": true,
                "intranetUser": true,
                "status": "online",
                "idle": false,
                "lastActivityDate": "2026-02-13T14:27:33+01:00",
                "mobileLastDate": false,
                "desktopLastDate": false,
                "absent": false,
                "departments": [1, 107, 47, 3],
                "phones": {
                    "personal_mobile": "19998887766",
                    "inner_phone": "111"
                },
                "botData": null,
                "type": "user",
                "website": "",
                "email": "user@example.com"
            }
        ],
        "files": [],
        "additionalMessages": [],
        "copilot": null,
        "stickers": [],
        "reactions": [
            {
                "messageId": 33653,
                "reactionCounters": {
                    "like": 1
                },
                "reactionUsers": {
                    "like": [1]
                },
                "ownReactions": ["like"]
            }
        ],
        "tariffRestrictions": {
            "isHistoryLimitExceeded": false
        },
        "usersShort": [
            {
                "id": 1,
                "name": "Alex",
                "avatar": "https://cdn-com.bitrix24.com/path/avatar.jpg",
                "color": "#df532d",
                "type": "user"
            }
        ],
        "messages": [
            {
                "id": 33653,
                "chatId": 3,
                "chat_id": 3,
                "authorId": 1,
                "author_id": 1,
                "date": "2026-02-13T14:28:00+01:00",
                "text": "test message",
                "isSystem": false,
                "uuid": "18533186-232b-4423-8438-64501da182f5",
                "forward": null,
                "params": {
                    "LIKE": [1]
                },
                "viewedByOthers": false,
                "block": null,
                "unread": false,
                "viewed": true
            }
        ]
    },
    "time": {
        "start": 1770982150,
        "finish": 1770982150.503861,
        "duration": 0.5038609504699707,
        "processing": 0,
        "date_start": "2026-02-13T14:29:10+01:00",
        "date_finish": "2026-02-13T14:29:10+01:00",
        "operating_reset_at": 1770982750,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Found messages and related data [(detailed description)](#result-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result-object}

#|
|| **Name**
`type` | **Description** ||
|| **messages**
[`array`](../../data-types.md) | Array of found messages [(detailed description)](#message) ||
|| **users**
[`array`](../../data-types.md) | Users associated with the found messages [(detailed description)](#user) ||
|| **files**
[`array`](../../data-types.md) | Files from the found messages. The element structure is the same as the [file object](./im-dialog-messages-get.md#file) in the response of the [im.dialog.messages.get](./im-dialog-messages-get.md) method. If there are no files, an empty array is returned ||
|| **additionalMessages**
[`array`](../../data-types.md) | Additional messages related to the found ones, such as forwarded or quoted [(detailed description)](#message) ||
|| **copilot**
[`object`](../../data-types.md) | BitrixGPT data. Can be `null` ||
|| **stickers**
[`array`](../../data-types.md) | Stickers associated with the found messages. If there are no stickers, an empty array is returned ||
|| **reactions**
[`array`](../../data-types.md) | Reactions to the found messages [(detailed description)](#reactions) ||
|| **tariffRestrictions**
[`object`](../../data-types.md) | History restrictions of the plan.

The `isHistoryLimitExceeded` flag is `true` if the plan's history restriction removed some messages from the selection ||
|| **usersShort**
[`array`](../../data-types.md) | Users who reacted: `id`, `name`, `avatar`, `color` — the avatar color, `type` — the user type.

Use this data to show names and avatars for the identifiers from `result.reactions[].reactionUsers` ||
|| **commentInfo**
[`array`](../../data-types.md) | Data about comments on the found messages. Populated for channel messages ||
|#

If nothing is found, all arrays of the `result` object are empty.

#### Message Object {#message}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the message ||
|| **chatId**
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **chat_id**
[`integer`](../../data-types.md) | Identifier of the chat. The same as `chatId`, using the legacy field name ||
|| **authorId**
[`integer`](../../data-types.md) | Identifier of the message author ||
|| **author_id**
[`integer`](../../data-types.md) | Identifier of the message author. The same as `authorId`, using the legacy field name ||
|| **date**
[`datetime`](../../data-types.md) | Date and time of message creation ||
|| **text**
[`string`](../../data-types.md) | Text of the message ||
|| **isSystem**
[`boolean`](../../data-types.md) | Indicator of a system message ||
|| **uuid**
[`string`](../../data-types.md) | External UUID of the message. Can be `null` ||
|| **forward**
[`object`](../../data-types.md) | Information about forwarding. `null` if the message is not forwarded ||
|| **params**
[`object`](../../data-types.md) | Message parameters. If there are no parameters, an empty array `[]` is returned ||
|| **viewedByOthers**
[`boolean`](../../data-types.md) | Indicator that the message has been viewed by other participants ||
|| **unread**
[`boolean`](../../data-types.md) | Indicator of an unread message for the current user ||
|| **viewed**
[`boolean`](../../data-types.md) | Indicator that the message has been viewed by the current user ||
|| **block**
[`object`](../../data-types.md) | Content blocks of the message. `null` if there are no blocks ||
|#

#### User Object {#user}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the user ||
|| **active**
[`boolean`](../../data-types.md) | User is active ||
|| **name**
[`string`](../../data-types.md) | Full name ||
|| **firstName**
[`string`](../../data-types.md) | First name ||
|| **lastName**
[`string`](../../data-types.md) | Last name ||
|| **workPosition**
[`string`](../../data-types.md) | Position ||
|| **color**
[`string`](../../data-types.md) | Profile color in hex format ||
|| **avatar**
[`string`](../../data-types.md) | Avatar URL ||
|| **avatarHr**
[`string`](../../data-types.md) | High-resolution avatar URL ||
|| **gender**
[`string`](../../data-types.md) | Gender ||
|| **birthday**
[`string`](../../data-types.md) | Birthday ||
|| **extranet**
[`boolean`](../../data-types.md) | Indicator of an extranet user ||
|| **intranetUser**
[`boolean`](../../data-types.md) | `true` if the user is an employee: they belong to a company department and their account is not external ||
|| **internalAccount**
[`boolean`](../../data-types.md) | `true` if the account is not external, for example, if it does not belong to an Open Channel client ||
|| **network**
[`boolean`](../../data-types.md) | Indicator of a Bitrix24 Network user ||
|| **bot**
[`boolean`](../../data-types.md) | Indicator of a bot ||
|| **connector**
[`boolean`](../../data-types.md) | Indicator of a connector user ||
|| **externalAuthId**
[`string`](../../data-types.md) | External authorization code ||
|| **status**
[`string`](../../data-types.md) | User status ||
|| **idle**
[`datetime`](../../data-types.md) | Date in ISO 8601 format if the user is idle, otherwise `false` ||
|| **lastActivityDate**
[`datetime`](../../data-types.md) | Date and time of last activity or `false` ||
|| **mobileLastDate**
[`datetime`](../../data-types.md) | Last activity in the mobile app. Can be `false` ||
|| **desktopLastDate**
[`datetime`](../../data-types.md) | Last activity in the desktop app. Can be `false` ||
|| **absent**
[`datetime`](../../data-types.md) | Date in ISO 8601 format if the user is absent, otherwise `false` ||
|| **departments**
[`array`](../../data-types.md) | Array of department identifiers ||
|| **phones**
[`object`](../../data-types.md) | User's phones or `false` if no phones are specified ||
|| **botData**
[`object`](../../data-types.md) | Bot data. For a regular user — `null` ||
|| **type**
[`string`](../../data-types.md) | User type ||
|| **website**
[`string`](../../data-types.md) | User's website ||
|| **email**
[`string`](../../data-types.md) | User's e-mail ||
|#

#### Reactions Object {#reactions}

#|
|| **Name**
`type` | **Description** ||
|| **messageId**
[`integer`](../../data-types.md) | Identifier of the message ||
|| **reactionCounters**
[`object`](../../data-types.md) | Number of reactions by type: the key is the reaction code, for example, `like`, and the value is the number of reactions ||
|| **reactionUsers**
[`object`](../../data-types.md) | Who reacted: the key is the reaction code, and the value is an array of user identifiers. If there are more than five reactions of one type, the list may be incomplete. Names and avatars of these users are in `usersShort` ||
|| **ownReactions**
[`array`](../../data-types.md) | Codes of reactions set by the current user, for example, `["like"]` ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "CHAT_ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID_EMPTY` | ``CHAT_ID can`t be empty`` | The `CHAT_ID` parameter is not passed or its value is not a number ||
|| `400` | `500` | `Incorrect date/time: <value>` | The `DATE_FROM`, `DATE_TO`, or `DATE` value is not recognized as a date ||
|| `400` | `ERROR_ARGUMENT` | Invalid order "UP" | The direction passed in `ORDER` is neither `ASC` nor `DESC`. The error text contains the passed value ||
|| `403` | `ACCESS_ERROR` | You do not have access to this chat | No access to the specified chat ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-message-add.md)
- [{#T}](./im-message-update.md)
- [{#T}](./im-message-delete.md)
- [{#T}](./im-message-like.md)
- [{#T}](./im-message-share.md)
- [{#T}](./im-message-command.md)
- [{#T}](./im-dialog-messages-get.md)
- [{#T}](./im-dialog-read.md)
- [{#T}](./im-dialog-unread.md)
- [{#T}](./im-dialog-writing.md)
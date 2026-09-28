# Get Information About the Chat imbot.v2.Chat.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.get` returns information about a chat the bot is a participant of. The method returns the data of an open chat or an open channel even if the bot is not a participant.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Bot token. Required for webhook authorization, not needed for OAuth.

Pass the same `botToken` that you specified when registering the bot ||
|| **dialogId***
[`string`](../../../../data-types.md) | Dialog ID in the [dialogId format](../../index.md#dialog-id): `chat{chatId}` for a group chat, `{userId}` for a personal chat ||
|#

## Code Examples

{% include [Examples Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat5"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat5","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.get', {
        botId: 456,
        dialogId: 'chat5',
      });

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imbot.v2.chat.get(
            bot_id=456,
            dialog_id="chat5",
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
                'imbot.v2.Chat.get',
                [
                    'botId' => 456,
                    'dialogId' => 'chat5',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: '. print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: '. $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.get',
        {
            botId: 456,
            dialogId: 'chat5',
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
        'imbot.v2.Chat.get',
        [
            'botId' => 456,
            'dialogId' => 'chat5',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'Chat name: '. $result['result']['chat']['name'];
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imbot.v2.Chat.get", b24.Params{
    	"botId":    456,
    	"botToken": "my_bot_token",
    	"dialogId": "chat5",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imbot.v2.Chat.get: %w", err)
    }

    // The method wraps the response in an object with the "chat" key.
    raw, ok := b24.Unwrap(res.Result, "chat")
    if !ok {
    	return fmt.Errorf("no chat key in the response")
    }

    var item struct {
    	ID          b24.ID `json:"id"`
    	DialogID    string `json:"dialogId"`
    	Name        string `json:"name"`
    	Description string `json:"description"`
    	Type        string `json:"type"`
    	MessageType string `json:"messageType"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.DialogID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "chat": {
            "id": 5,
            "dialogId": "chat5",
            "name": "Support Chat",
            "description": "",
            "type": "chat",
            "messageType": "C",
            "owner": 456,
            "color": "#4ba984",
            "avatar": "",
            "extranet": false,
            "containsCollaber": false,
            "entityType": "",
            "entityId": "",
            "entityData1": "",
            "entityData2": "",
            "entityData3": "",
            "entityLink": {
                "type": "",
                "url": "",
                "id": ""
            },
            "diskFolderId": null,
            "role": "owner",
            "permissions": {
                "manageUsersAdd": "member",
                "manageUsersDelete": "manager",
                "manageUi": "member",
                "manageSettings": "owner",
                "manageMessages": "member",
                "manageMessagesAutoDelete": "manager",
                "manageGuestInvites": "manager",
                "manageDelete": "member",
                "canPost": "member"
            },
            "hasManageCapability": false,
            "canHaveThreads": true,
            "muteList": [],
            "parentChatId": null,
            "parentMessageId": null,
            "isNew": false,
            "textFieldEnabled": true,
            "backgroundId": null,
            "dateCreate": "2025-01-15T10:00:00+01:00",
            "lastMessageId": 789,
            "lastMessageViews": {
                "messageId": 789,
                "firstViewers": [],
                "countOfViewers": 0
            },
            "lastId": 789,
            "managerList": [456],
            "markedId": 0,
            "messageCount": 15,
            "public": "",
            "unreadId": 0,
            "userCounter": 3,
            "guestCount": 0
        },
        "users": [
            {
                "id": 456,
                "active": true,
                "name": "Support Bot",
                "firstName": "Support Bot",
                "lastName": "",
                "workPosition": "",
                "color": "#4ba984",
                "avatar": "",
                "gender": "M",
                "birthday": "",
                "extranet": false,
                "bot": true,
                "connector": false,
                "externalAuthId": "bot",
                "status": "online",
                "idle": false,
                "lastActivityDate": false,
                "mobileLastDate": false,
                "desktopLastDate": false,
                "absent": false,
                "departments": [],
                "phones": false,
                "type": "bot",
                "website": "",
                "email": ""
            }
        ]
    },
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+01:00",
        "date_finish": "2024-10-11T10:00:00+01:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Result of the request ||
|| **result.chat**
[`Chat`](../../entities.md#chat) | Chat object [(detailed description)](#chat-object) ||
|| **result.users**
[`User[]`](../../entities.md#user) | An array with a single element — the data of the bot on whose behalf the request was made. Chat participants are returned by [imbot.v2.Chat.User.list](./chat-user-list.md). Field descriptions — [User](../../entities.md#user) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

In addition to `chat` and `users`, the response contains service keys of the messenger interface: `recentConfig`, `parentChat`, `copilot`, `messagesAutoDeleteConfigs`, and `callInfo`. A bot does not need them, so they are omitted from the response example.

### Chat Object Fields {#chat-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Chat identifier ||
|| **dialogId**
[`string`](../../../../data-types.md) | Dialog identifier. For a group chat — `chat{id}`, for example `chat5` ||
|| **name**
[`string`](../../../../data-types.md) | Name of the chat ||
|| **description**
[`string`](../../../../data-types.md) | Chat description. An empty string if not set ||
|| **type**
[`string`](../../../../data-types.md) | Type of chat: `chat`, `open`, `channel`, `openChannel`, `copilot`, and others — [list of values](../../entities.md#chat) ||
|| **messageType**
[`string`](../../../../data-types.md) | Internal single-letter chat type, for example `C` for a group chat and `O` for an open chat ||
|| **owner**
[`integer`](../../../../data-types.md) | ID of the chat owner ||
|| **color**
[`string`](../../../../data-types.md) | Chat color in HEX format ||
|| **avatar**
[`string`](../../../../data-types.md) | URL of the chat avatar. An empty string if not set ||
|| **extranet**
[`boolean`](../../../../data-types.md) | Whether the chat has extranet users ||
|| **containsCollaber**
[`boolean`](../../../../data-types.md) | Whether the chat has collabers ||
|| **entityType**
[`string`](../../../../data-types.md) | Type of the linked object, for example `LINES` for Open Channels. An empty string if the chat is not linked to an object ||
|| **entityId**
[`string`](../../../../data-types.md) | Identifier of the linked object ||
|| **entityData1**
[`string`](../../../../data-types.md) | Additional data of the linked object, field 1 ||
|| **entityData2**
[`string`](../../../../data-types.md) | Additional data of the linked object, field 2 ||
|| **entityData3**
[`string`](../../../../data-types.md) | Additional data of the linked object, field 3 ||
|| **entityLink**
[`object`](../../../../data-types.md) | Link to the linked object — an object with the keys `type`, `url`, and `id`. If the chat is not linked to an object, the values are empty ||
|| **diskFolderId**
[```integer|null```](../../../../data-types.md) | ID of the Drive folder that stores the chat files ||
|| **role**
[`string`](../../../../data-types.md) | Role of the bot in the chat: `owner`, `manager`, `member`, or `guest`. A bot that is not a participant of an open chat has the `guest` role ||
|| **permissions**
[`object`](../../../../data-types.md) | Minimum role for actions in the chat. Keys: `manageUsersAdd`, `manageUsersDelete`, `manageUi`, `manageSettings`, `manageMessages`, `manageMessagesAutoDelete`, `manageGuestInvites`, `manageDelete`, `canPost`. Values: `member`, `manager`, `owner`, or `none` — the action is unavailable to everyone ||
|| **canHaveThreads**
[`boolean`](../../../../data-types.md) | Whether threads can be created in the chat ||
|| **hasManageCapability**
[`boolean`](../../../../data-types.md) | Service flag of extended access to chat management ||
|| **muteList**
[`integer[]`](../../../../data-types.md) | Contains the bot ID if the bot has turned off notifications in the chat, otherwise an empty array ||
|| **parentChatId**
[```integer|null```](../../../../data-types.md) | ID of the parent chat if this is a thread ||
|| **parentMessageId**
[```integer|null```](../../../../data-types.md) | ID of the parent message if this is a thread ||
|| **isNew**
[`boolean`](../../../../data-types.md) | `true` for an open channel created less than 24 hours ago. `false` for all other chats ||
|| **textFieldEnabled**
[`boolean`](../../../../data-types.md) | Whether the message input field is enabled ||
|| **backgroundId**
[```string|null```](../../../../data-types.md) | Chat background ID ||
|| **dateCreate**
[```string|null```](../../../../data-types.md) | Creation date of the chat in ISO 8601 format ||
|| **lastMessageId**
[```integer|null```](../../../../data-types.md) | ID of the last message ||
|| **lastMessageViews**
[`object`](../../../../data-types.md) | Views of the last message: `messageId` — message ID, `firstViewers` — the first viewers, `countOfViewers` — the number of viewers ||
|| **lastId**
[`integer`](../../../../data-types.md) | ID of the last message read by the bot ||
|| **managerList**
[`integer[]`](../../../../data-types.md) | IDs of the chat managers ||
|| **markedId**
[`integer`](../../../../data-types.md) | ID of the message the bot marked as unread. `0` if there is no mark ||
|| **messageCount**
[`integer`](../../../../data-types.md) | Number of messages in the chat ||
|| **public**
[```string|object```](../../../../data-types.md) | Public link to the chat — an object with the fields `code` and `link`. An empty string if there is no link ||
|| **unreadId**
[`integer`](../../../../data-types.md) | ID of the first message unread by the bot. `0` if there are no unread messages ||
|| **userCounter**
[`integer`](../../../../data-types.md) | Number of chat participants ||
|| **guestCount**
[`integer`](../../../../data-types.md) | Number of guests in the chat ||
|#

Which of these fields are included in event data is described on the page [Objects and Fields — Chat](../../entities.md#chat).

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "ACCESS_DENIED"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token not specified (botToken is required for webhook auth) | `botToken` is not specified. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | botId is required | `botId` is not specified ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot was installed by another rest application | The bot is registered by another application ||
|| `CHAT_NOT_FOUND` | CHAT_NOT_FOUND | No chat found with the specified `dialogId` ||
|| `ACCESS_DENIED` | ACCESS_DENIED | The bot is not a participant of a private chat ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](./chat-add.md)
- [{#T}](./chat-update.md)
- [{#T}](./chat-user-list.md)
- [{#T}](./index.md)
- [{#T}](../../migration.md)
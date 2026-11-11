# Chatbot Objects and Fields 2.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Users, bots, chats, messages, files, and commands are returned in the responses of the `im.v2` and `imbot.v2` methods, in event data, and in webhook notifications.

> Quick navigation: [User](#user) | [Bot](#bot) | [Chat](#chat) | [Message](#message) | [File](#file) | [Command](#command)

Field types depend on where the object comes from:

- a method response or an event from `Event.get` in `fetch` mode — types are preserved as is
- an event in `webhook` mode — all scalar values come as strings: `integer` → `"123"`, `boolean` → `"1"`/`"0"`. Fields with a `null` value and empty arrays are not included in the event data

In events, the online fields of the User object — `idle`, `lastActivityDate`, `mobileLastDate`, `desktopLastDate` — are always `false`.

## User {#user}

A user of the system. Returned in the `user` fields and in the `users` collections.

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the user ||
|| **active**
[`boolean`](../../data-types.md) | Is the user active in the system? ||
|| **name**
[`string`](../../data-types.md) | Full name ||
|| **firstName**
[```string|null```](../../data-types.md) | First name. `null` for the author of system messages ||
|| **lastName**
[```string|null```](../../data-types.md) | Last name. `null` for the author of system messages ||
|| **workPosition**
[```string|null```](../../data-types.md) | Job title, or `null` if it is not specified ||
|| **color**
[`string`](../../data-types.md) | Avatar placeholder color in HEX format, e.g., `#df532d` ||
|| **avatar**
[`string`](../../data-types.md) | Avatar URL. An empty string if no avatar is set ||
|| **gender**
[`string`](../../data-types.md) | Gender: `M` — male, `F` — female ||
|| **birthday**
[`string`](../../data-types.md) | Date of birth. An empty string if not specified ||
|| **extranet**
[`boolean`](../../data-types.md) | Is the user an extranet user? ||
|| **bot**
[`boolean`](../../data-types.md) | Is the user a bot? ||
|| **connector**
[`boolean`](../../data-types.md) | Is the user a connector for Open Channels? ||
|| **externalAuthId**
[`string`](../../data-types.md) | Type of external authorization: `default`, `bot`, `email`, `replica`, and others ||
|| **status**
[`string`](../../data-types.md) | User status. In `imbot.v2` method responses and in events, it is always `online` ||
|| **idle**
[```string | boolean```](../../data-types.md#compound-types) | Time of inactivity in ISO 8601 format, or `false` ||
|| **lastActivityDate**
[```string | boolean```](../../data-types.md#compound-types) | Date of last activity in ISO 8601 format, or `false` ||
|| **mobileLastDate**
[```string | boolean```](../../data-types.md#compound-types) | Date of last mobile login in ISO 8601 format, or `false` ||
|| **desktopLastDate**
[```string | boolean```](../../data-types.md#compound-types) | Date of last desktop login in ISO 8601 format, or `false` ||
|| **absent**
[```string | boolean```](../../data-types.md#compound-types) | Date and time of return from vacation in ISO 8601 format. This is the day after the vacation ends. `false` if the user is not on vacation. Other types of absence, such as a business trip, are not taken into account ||
|| **departments**
[`integer[]`](../../data-types.md) | Array of department IDs ||
|| **phones**
[```object | boolean```](../../data-types.md#compound-types) | User phone numbers, or `false` if the user has no phone numbers. The object contains only the filled-in numbers. Possible keys: `personal_mobile`, `work_phone`, `inner_phone`, `personal_phone` ||
|| **type**
[`string`](../../data-types.md) | User type, for example `user` — an employee, `extranet` — an external user, `bot` — a chatbot ||
|| **internalAccount**
[`boolean`](../../data-types.md) | Internal Bitrix24 account ||
|| **intranetUser**
[`boolean`](../../data-types.md) | Intranet user — a company employee ||
|| **website**
[`string`](../../data-types.md) | Personal website ||
|| **email**
[`string`](../../data-types.md) | Email ||
|#

### Example User Object

The example does not show all fields.

```json
{
    "id": 1,
    "active": true,
    "name": "John Smith",
    "firstName": "John",
    "lastName": "Smith",
    "workPosition": "Developer",
    "color": "#df532d",
    "avatar": "",
    "gender": "M",
    "birthday": "",
    "extranet": false,
    "bot": false,
    "connector": false,
    "externalAuthId": "default",
    "status": "online",
    "idle": false,
    "lastActivityDate": "2025-01-15T10:29:00+01:00",
    "absent": false,
    "departments": [1, 5],
    "phones": false,
    "type": "user",
    "website": "",
    "email": "john@example.com"
}
```

## Bot {#bot}

Chatbot. Returned in the `bot` field of the [imbot.v2.Bot.get](./imbot.v2/bots/bot-get.md) response, in the `bots` array of the [imbot.v2.Bot.list](./imbot.v2/bots/bot-list.md) response, and in event data.

The object comes in two formats: the brief one contains common fields, the extended one also contains fields visible only to the bot owner. Events in `webhook` mode bring a third format with the `id`, `code`, and `auth` fields. It is described in the [Bot Object Format](./imbot.v2/events/events.md#bot-format) section.

### Common Fields

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | ID of the bot user ||
|| **code**
[`string`](../../data-types.md) | Unique string code of the bot ||
|| **type**
[`string`](../../data-types.md) | Type of the bot. Description of types — [Bot Types](./index.md#bot-types) ||
|| **isHidden**
[`boolean`](../../data-types.md) | Is the bot hidden from the contact list? ||
|| **isSupportOpenline**
[`boolean`](../../data-types.md) | Does the bot support working with Open Channels? ||
|| **isReactionsEnabled**
[`boolean`](../../data-types.md) | Are reactions to bot messages enabled? ||
|| **backgroundId**
[```string|null```](../../data-types.md) | ID of the bot chat background or `null` ||
|| **language**
[`string`](../../data-types.md) | Default language of the bot, e.g., `en`, `de` ||
|#

### Additional Fields (for owner only)

#|
|| **Field**
`Type` | **Description** ||
|| **moduleId**
[`string`](../../data-types.md) | ID of the module to which the bot belongs ||
|| **eventMode**
[`string`](../../data-types.md) | Event delivery mode: `webhook` or `fetch` ||
|| **countMessage**
[`integer`](../../data-types.md) | Internal counter, see the note below the table ||
|| **countCommand**
[`integer`](../../data-types.md) | Internal counter. It does not equal the number of registered commands ||
|| **countChat**
[`integer`](../../data-types.md) | Internal counter ||
|| **countUser**
[`integer`](../../data-types.md) | Internal counter ||
|#

Bitrix24 does not update the `countMessage`, `countCommand`, `countChat`, and `countUser` counters for every action, and updates them with a delay. After messages and a command call, the values may remain `0`. To count the bot's messages, commands, or chats, keep track on your side using events.

### Example Bot Object (extended format)

```json
{
    "id": 456,
    "code": "support_bot",
    "type": "bot",
    "isHidden": false,
    "isSupportOpenline": false,
    "isReactionsEnabled": true,
    "backgroundId": null,
    "language": "en",
    "moduleId": "rest",
    "eventMode": "fetch",
    "countMessage": 150,
    "countCommand": 3,
    "countChat": 12,
    "countUser": 45
}
```

## Chat {#chat}

Chat. Returned in the `chat` field of the responses from the methods [imbot.v2.Chat.get](./imbot.v2/chats/chat-get.md), [imbot.v2.Chat.add](./imbot.v2/chats/chat-add.md), as well as in event data.

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the chat ||
|| **dialogId**
[`string`](../../data-types.md) | Dialog identifier: `chat5` for group chats, `123` for personal chats ||
|| **name**
[`string`](../../data-types.md) | Name of the chat ||
|| **type**
[`string`](../../data-types.md) | Chat type, for example `private` — a one-on-one chat, `chat` — a group chat, `open`, `channel`, `openChannel`, `copilot`, `generalChannel` ||
|| **messageType**
[`string`](../../data-types.md) | Internal type: `C` (chat), `O` (open), `P` (private), and others ||
|| **owner**
[`integer`](../../data-types.md) | ID of the chat owner ||
|| **color**
[```string|null```](../../data-types.md) | Color of the chat in HEX format ||
|| **avatar**
[`string`](../../data-types.md) | URL of the chat avatar. An empty string if not set ||
|| **description**
[`string`](../../data-types.md) | Description of the chat ||
|| **extranet**
[`boolean`](../../data-types.md) | Does the chat contain extranet users? ||
|| **role**
[`string`](../../data-types.md) | Role of the current user: `owner`, `manager`, `member`, `guest`, `none`. Absent in events ||
|| **containsCollaber**
[`boolean`](../../data-types.md) | Does the chat contain collaborators? ||
|| **muteList**
[`array`](../../data-types.md) | ID of the current user if they have muted the chat notifications, otherwise an empty array. IDs of other participants are not included in the list. Absent in events ||
|| **entityType**
[`string`](../../data-types.md) | Type of the object the chat is linked to, for example `LINES` for Open Channels ||
|| **entityId**
[`string`](../../data-types.md) | Identifier of the object the chat is linked to ||
|| **entityData1**
[`string`](../../data-types.md) | Additional data of the object (field 1) ||
|| **entityData2**
[`string`](../../data-types.md) | Additional data of the object (field 2) ||
|| **entityData3**
[`string`](../../data-types.md) | Additional data of the object (field 3) ||
|| **entityLink**
[`object`](../../data-types.md) | Link to the object the chat is linked to: `type` — object type, `url` — object address, `id` — object ID. If the chat is not linked, all three fields are empty strings ||
|| **diskFolderId**
[```integer|null```](../../data-types.md) | ID of the folder on Drive for chat files ||
|| **permissions**
[`object`](../../data-types.md) | Permissions in the chat. [(detailed description)](#chat-permissions) ||
|| **parentChatId**
[```integer|null```](../../data-types.md) | ID of the parent chat for threads ||
|| **parentMessageId**
[```integer|null```](../../data-types.md) | ID of the parent message for threads ||
|| **isNew**
[`boolean`](../../data-types.md) | Is the chat newly created? ||
|| **textFieldEnabled**
[`boolean`](../../data-types.md) | Whether the text input field is enabled ||
|| **backgroundId**
[```string|null```](../../data-types.md) | ID of the chat background or `null` ||
|| **hasManageCapability**
[`boolean`](../../data-types.md) | Internal interface field ||
|| **canHaveThreads**
[`boolean`](../../data-types.md) | Whether threads can be created in the chat ||
|#

### permissions Object {#chat-permissions}

Each key is a group of actions in the chat. The value is the minimum role allowed to perform these actions: `member` — a participant, `manager` — a chat manager, `owner` — the chat owner, `none` — the actions are forbidden to everyone.

#|
|| **Field**
`Type` | **Description** ||
|| **manageUsersAdd**
[`string`](../../data-types.md) | Add participants ||
|| **manageUsersDelete**
[`string`](../../data-types.md) | Remove participants ||
|| **manageUi**
[`string`](../../data-types.md) | Change the chat name, description, color, and avatar ||
|| **manageSettings**
[`string`](../../data-types.md) | Change the chat permissions, owner, and managers ||
|| **manageMessages**
[`string`](../../data-types.md) | Post and pin messages ||
|| **manageMessagesAutoDelete**
[`string`](../../data-types.md) | Configure auto-deletion of messages ||
|| **manageGuestInvites**
[`string`](../../data-types.md) | Invite guests ||
|| **manageDelete**
[`string`](../../data-types.md) | Delete the chat ||
|| **canPost**
[`string`](../../data-types.md) | Same value as `manageMessages` ||
|#

### Additional Fields (only in method responses)

Fields that are returned in method responses (e.g., [imbot.v2.Chat.get](./imbot.v2/chats/chat-get.md)), but not passed in events.

#|
|| **Field**
`Type` | **Description** ||
|| **dateCreate**
[```string|null```](../../data-types.md) | Date of chat creation in ISO 8601 format ||
|| **lastMessageId**
[```integer|null```](../../data-types.md) | ID of the last message ||
|| **lastId**
[`integer`](../../data-types.md) | ID of the last read message. `0` if there are no read messages ||
|| **managerList**
[`array`](../../data-types.md) | Array of chat manager IDs ||
|| **messageCount**
[`integer`](../../data-types.md) | Number of messages in the chat ||
|| **userCounter**
[`integer`](../../data-types.md) | Number of participants in the chat ||
|| **unreadId**
[`integer`](../../data-types.md) | ID of the first unread message. `0` if there are no unread messages ||
|| **lastMessageViews**
[`object`](../../data-types.md) | Who has viewed the last message in the chat: `messageId` — ID of that message, `firstViewers` — an array of the first viewers, `countOfViewers` — how many users have viewed it ||
|| **markedId**
[```integer|null```](../../data-types.md) | ID of the marked message ||
|| **public**
[```object|string```](../../data-types.md) | Public access settings ||
|#

### Example Chat Object

The example does not show all fields.

```json
{
    "id": 5,
    "dialogId": "chat5",
    "name": "Support Chat",
    "type": "chat",
    "messageType": "C",
    "owner": 1,
    "color": "#ab7761",
    "avatar": "",
    "description": "",
    "extranet": false,
    "role": "member",
    "containsCollaber": false,
    "diskFolderId": 42,
    "textFieldEnabled": true,
    "backgroundId": null,
    "dateCreate": "2025-01-10T09:00:00+01:00",
    "lastMessageId": 789,
    "managerList": [1, 3],
    "messageCount": 42,
    "userCounter": 5,
    "unreadId": 0
}
```

## Message {#message}

Message. Returned in the `message` field of method responses and event data.

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the message ||
|| **chatId**
[`integer`](../../data-types.md) | ID of the chat ||
|| **authorId**
[`integer`](../../data-types.md) | ID of the author. `0` for system messages in the chat, for example about creating the chat or adding a participant ||
|| **date**
[```string|null```](../../data-types.md) | Creation date in ISO 8601 format ||
|| **text**
[`string`](../../data-types.md) | Message text. Text longer than 20,006 characters is truncated to 20,000 characters, and ` (...)` is appended at the end ||
|| **isSystem**
[`boolean`](../../data-types.md) | Is the message a system message? ||
|| **uuid**
[```string|null```](../../data-types.md) | Message UUID if it was passed when sending, or `null` ||
|| **forward**
[```object|null```](../../data-types.md) | Forwarding data if the message was forwarded, or `null`. `id` — ID of the original message in the `<chat>/<message>` format, `userId` — ID of the author of the original message. The other fields describe the original chat: `chatTitle`, `chatType`, `parentChatId`, `parentMessageId`, `parentChatType` ||
|| **params**
[```object|array```](../../data-types.md) | Additional message parameters: attachments, keyboard, files, and others. If there are no parameters, an empty array `[]` is returned, and in `webhook` mode events the field is absent. [(detailed description)](#message-params) ||
|| **viewedByOthers**
[`boolean`](../../data-types.md) | Has the message been read by other participants? ||
|#

### params Object {#message-params}

`params` contains only the keys set for the message. The main ones:

#|
|| **Field**
`Type` | **Description** ||
|| **ATTACH**
[`array`](../../data-types.md) | Attachments — cards with properties, links, and images: an array of objects with the `ID`, `BLOCKS`, `DESCRIPTION`, `COLOR`, `COLOR_TOKEN` fields. The blocks are described in the [Attachments](./imbot.v2/messages/attachments/index.md) section ||
|| **KEYBOARD**
[`array`](../../data-types.md) | Keyboard buttons under the message: an array of objects with the `TEXT`, `LINK`, `BG_COLOR_TOKEN`, `BOT_ID`, `TYPE`, `DISABLED` fields, and others ||
|| **FILE_ID**
[`array`](../../data-types.md) | IDs of the message files: an array of strings ||
|| **REPLY_ID**
[`string`](../../data-types.md) | ID of the original message as a string if this message is a reply ||
|| **LIKE**
[`array`](../../data-types.md) | Array of IDs of users who added a "like" reaction ||
|| **IS_EDITED**
[`string`](../../data-types.md) | `Y` if the message was edited ||
|| **IS_DELETED**
[`string`](../../data-types.md) | `Y` if the message was deleted ||
|#

### Additional Fields (only in method responses)

#|
|| **Field**
`Type` | **Description** ||
|| **unread**
[`boolean`](../../data-types.md) | Unread for the current user ||
|| **viewed**
[`boolean`](../../data-types.md) | Viewed by the current user ||
|#

### Example Message Object

The example does not show all fields.

```json
{
    "id": 789,
    "chatId": 5,
    "authorId": 1,
    "date": "2025-01-15T10:30:00+01:00",
    "text": "Hello bot!",
    "isSystem": false,
    "uuid": null,
    "forward": null,
    "params": [],
    "viewedByOthers": false
}
```

## File {#file}

File attached to a message. Returned in the `file` field of the response from the method [imbot.v2.File.upload](./imbot.v2/files/file-upload.md).

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | ID of the file on Drive ||
|| **chatId**
[`integer`](../../data-types.md) | ID of the chat ||
|| **date**
[```string|null```](../../data-types.md) | Upload date in ISO 8601 format ||
|| **type**
[`string`](../../data-types.md) | Content type: `file`, `image`, `video`, `audio` ||
|| **name**
[`string`](../../data-types.md) | File name with extension ||
|| **extension**
[`string`](../../data-types.md) | File extension in lowercase ||
|| **size**
[`integer`](../../data-types.md) | File size in bytes ||
|| **duration**
[```integer|null```](../../data-types.md) | Duration of the audio or video. `null` for other files ||
|| **image**
[```object | boolean```](../../data-types.md#compound-types) | Preview sizes for images: `{"height": 600, "width": 800}`, or `false` ||
|| **authorId**
[`integer`](../../data-types.md) | ID of the user who uploaded the file ||
|| **authorName**
[`string`](../../data-types.md) | Name of the user who uploaded the file ||
|| **isTranscribable**
[`boolean`](../../data-types.md) | Is transcription of the file possible? ||
|| **isVideoNote**
[`boolean`](../../data-types.md) | Is the file a video note? ||
|| **isVoiceNote**
[`boolean`](../../data-types.md) | Is the file a voice message? ||
|#

### Example File Object

```json
{
    "id": 138,
    "chatId": 5,
    "date": "2025-01-15T10:30:00+01:00",
    "type": "file",
    "name": "report.pdf",
    "extension": "pdf",
    "size": 35341,
    "duration": null,
    "image": false,
    "authorId": 1,
    "authorName": "John Smith",
    "isTranscribable": false,
    "isVideoNote": false,
    "isVoiceNote": false
}
```

## Command {#command}

A bot slash command is a command that a user invokes in a chat by typing a slash, for example `/help`. The set of fields depends on where the object comes from: in the response to registering or updating a command, in the list of commands, or in a command call event.

### In register and update Responses {#command-register}

The `command` field of the [imbot.v2.Command.register](./imbot.v2/commands/command-register.md) and [imbot.v2.Command.update](./imbot.v2/commands/command-update.md) responses.

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Command ID ||
|| **botId**
[`integer`](../../data-types.md) | ID of the bot that owns the command ||
|| **command**
[`string`](../../data-types.md) | Command text with leading slash, e.g., `/help` ||
|| **common**
[`boolean`](../../data-types.md) | Is the command available in all chats (`true`) or only where the bot is present (`false`)? ||
|| **hidden**
[`boolean`](../../data-types.md) | Is the command hidden from the suggestion list? ||
|| **extranetSupport**
[`boolean`](../../data-types.md) | Is the command available for extranet users? ||
|#

Object example:

```json
{
    "id": 78,
    "botId": 456,
    "command": "/help",
    "common": false,
    "hidden": false,
    "extranetSupport": false
}
```

### In the list Response {#command-list}

Elements of the `commands` array of the [imbot.v2.Command.list](./imbot.v2/commands/command-list.md) response. The list also includes built-in Bitrix24 commands, for example `/me`: their `id` is a string like `def0`, and `botId` is `0`.

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[```integer|string```](../../data-types.md) | Command ID. A string for built-in commands ||
|| **botId**
[`integer`](../../data-types.md) | ID of the bot that owns the command. `0` for built-in commands ||
|| **command**
[`string`](../../data-types.md) | Command text with a leading slash ||
|| **category**
[`string`](../../data-types.md) | Name of the bot that owns the command, or the section of built-in commands ||
|| **common**
[`string`](../../data-types.md) | Whether the command is available in all chats: `Y` or `N` ||
|| **context**
[`string`](../../data-types.md) | Command context. An empty string for commands registered via REST ||
|| **title**
[`string`](../../data-types.md) | Command title in the language of the Bitrix24 account ||
|| **params**
[`string`](../../data-types.md) | Description of the command parameters in the language of the Bitrix24 account ||
|| **extranet**
[`string`](../../data-types.md) | Whether the command is available to extranet users: `Y` or `N` ||
|#

Array element example:

```json
{
    "id": 78,
    "botId": 456,
    "command": "/help",
    "category": "Support Bot",
    "common": "N",
    "context": "",
    "title": "Show help",
    "params": "query",
    "extranet": "N"
}
```

### In the Command Call Event {#command-event}

In the [ONIMBOTV2COMMANDADD](./imbot.v2/events/events.md#onimbotv2commandadd) event, the `command` object contains the `id`, `command`, `params`, and `context` fields. `params` contains the arguments the user passed when calling the command, and `context` shows how the command was called. The fields are described in the [Command Object](./imbot.v2/events/events.md#command-object) section.

## Continue Learning

- [API imbot.v2 Change Log](./change-log.md)
- [{#T}](./index.md)
- [{#T}](./imbot.v2/bots/index.md)
- [{#T}](./imbot.v2/events/events.md)
- [{#T}](./im.v2/events/events.md)
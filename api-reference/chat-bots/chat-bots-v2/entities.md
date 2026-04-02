# Objects and Fields of Chatbots 2.0

Description of the objects returned in the responses of the `im.v2` and `imbot.v2` methods, as well as in event data and webhook notifications.

> Quick navigation: [User](#user) | [Bot](#bot) | [Chat](#chat) | [Message](#message) | [File](#file) | [Command](#command)

Field types depend on the context:

— in method responses and FETCH events of `Event.get`, types are preserved as they are  
— in webhook events, all scalar values come as strings due to serialization via `http_build_query`: `integer` → `"123"`, `boolean` → `"1"`/`"0"`, `null` → `""`  
— in online events, the fields of the User object `idle`, `lastActivityDate`, `mobileLastDate`, `desktopLastDate` are always equal to `false`

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
[`string`](../../data-types.md) | First name ||
|| **lastName**  
[`string`](../../data-types.md) | Last name ||
|| **workPosition**  
[`string`](../../data-types.md) | Job title ||
|| **color**  
[`string`](../../data-types.md) | Placeholder avatar color in HEX format, e.g., `#df532d` ||
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
[`string`](../../data-types.md) | Type of external authentication: `default`, `bot`, `email`, `replica`, etc. ||
|| **status**  
[`string`](../../data-types.md) | Status: `online`, `dnd` ||
|| **idle**  
[`string\|false`](../../data-types.md) | Start time of inactivity in ISO 8601 format, or `false` ||
|| **lastActivityDate**  
[`string\|false`](../../data-types.md) | Date of last activity in ISO 8601 format, or `false` ||
|| **mobileLastDate**  
[`string\|false`](../../data-types.md) | Date of last mobile login in ISO 8601 format, or `false` ||
|| **desktopLastDate**  
[`string\|false`](../../data-types.md) | Date of last desktop login in ISO 8601 format, or `false` ||
|| **absent**  
[`string\|false`](../../data-types.md) | Start date of absence in ISO 8601 format, or `false` ||
|| **departments**  
[`integer[]`](../../data-types.md) | Array of department IDs ||
|| **phones**  
[`object\|false`](../../data-types.md) | Object with phone numbers (`personalPhone`, `workPhone`, etc.) or `false` ||
|| **type**  
[`string`](../../data-types.md) | Type: `employee`, `extranet`, `email`, `collaber`, `bot` ||
|| **website**  
[`string`](../../data-types.md) | Personal website ||
|| **email**  
[`string`](../../data-types.md) | Email ||
|#

### Example User Object

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
    "type": "employee",
    "website": "",
    "email": "john@example.com"
}
```

## Bot {#bot}

Chatbot. Returned in the `bot` field of the responses from the methods [imbot.v2.Bot.get](./imbot.v2/bots/bot-get.md), [imbot.v2.Bot.list](./imbot.v2/bots/bot-list.md), as well as in event data.

It has two formats: brief (public) and extended (for the bot owner only).

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
[`string\|null`](../../data-types.md) | ID of the bot chat background or `null` ||
|| **language**  
[`string`](../../data-types.md) | Default language of the bot, e.g., `en`, `de` ||
|#

### Additional Fields (for owner only)

#|
|| **Field**  
`Type` | **Description** ||
|| **moduleId**  
[`string`](../../data-types.md) | ID of the module to which the bot belongs ||
|| **appId**  
[`string`](../../data-types.md) | ID of the application that registered the bot ||
|| **eventMode**  
[`string`](../../data-types.md) | Event delivery mode: `webhook` or `fetch` ||
|| **countMessage**  
[`integer`](../../data-types.md) | Number of processed messages ||
|| **countCommand**  
[`integer`](../../data-types.md) | Number of registered commands ||
|| **countChat**  
[`integer`](../../data-types.md) | Number of chats the bot is in ||
|| **countUser**  
[`integer`](../../data-types.md) | Number of unique users ||
|#

### Example Bot Object (Extended Format)

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
    "appId": "custom123abc",
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
[`string`](../../data-types.md) | Identifier of the dialog: `chat5` for group chats, `123` for personal chats ||
|| **name**  
[`string`](../../data-types.md) | Name of the chat ||
|| **type**  
[`string`](../../data-types.md) | Type: `chat`, `open`, `channel`, `openChannel`, `copilot`, `thread`, `generalChannel` ||
|| **messageType**  
[`string`](../../data-types.md) | Internal type: `C` (chat), `O` (open), `P` (private), etc. ||
|| **owner**  
[`integer`](../../data-types.md) | ID of the chat owner ||
|| **color**  
[`string\|null`](../../data-types.md) | Color of the chat in HEX format ||
|| **avatar**  
[`string`](../../data-types.md) | URL of the chat avatar. An empty string if not set ||
|| **description**  
[`string`](../../data-types.md) | Description of the chat ||
|| **extranet**  
[`boolean`](../../data-types.md) | Does the chat contain extranet users? ||
|| **role**  
[`string`](../../data-types.md) | Role of the current user: `owner`, `manager`, `member`, `guest`, `none` ||
|| **containsCollaber**  
[`boolean`](../../data-types.md) | Does the chat contain collaborators? ||
|| **muteList**  
[`array`](../../data-types.md) | List of user IDs who have disabled notifications. Absent in events — depends on the specific user ||
|| **entityType**  
[`string`](../../data-types.md) | Type of the object, e.g., `LINES` for Open Channels ||
|| **entityId**  
[`string`](../../data-types.md) | Identifier of the element ||
|| **entityData1**  
[`string`](../../data-types.md) | Additional data of the object (field 1) ||
|| **entityData2**  
[`string`](../../data-types.md) | Additional data of the object (field 2) ||
|| **entityData3**  
[`string`](../../data-types.md) | Additional data of the object (field 3) ||
|| **entityLink**  
[`object`](../../data-types.md) | Data link to an external object ||
|| **diskFolderId**  
[`integer\|null`](../../data-types.md) | ID of the folder on Drive for chat files ||
|| **permissions**  
[`object`](../../data-types.md) | Access permissions for the current user ||
|| **parentChatId**  
[`integer\|null`](../../data-types.md) | ID of the parent chat for threads ||
|| **parentMessageId**  
[`integer\|null`](../../data-types.md) | ID of the parent message for threads ||
|| **isNew**  
[`boolean`](../../data-types.md) | Is the chat newly created? ||
|| **textFieldEnabled**  
[`string`](../../data-types.md) | Is the text input field enabled: `Y` or `N` ||
|| **backgroundId**  
[`string\|null`](../../data-types.md) | ID of the chat background or `null` ||
|#

### Additional Fields (Only in Method Responses)

Fields that are returned in method responses (e.g., [imbot.v2.Chat.get](./imbot.v2/chats/chat-get.md)), but are not passed in events.

#|
|| **Field**  
`Type` | **Description** ||
|| **dateCreate**  
[`string\|null`](../../data-types.md) | Date of chat creation in ISO 8601 format ||
|| **lastMessageId**  
[`integer\|null`](../../data-types.md) | ID of the last message ||
|| **lastId**  
[`integer\|null`](../../data-types.md) | ID of the last read message ||
|| **managerList**  
[`array`](../../data-types.md) | Array of chat manager IDs ||
|| **messageCount**  
[`integer`](../../data-types.md) | Number of messages in the chat ||
|| **userCounter**  
[`integer`](../../data-types.md) | Number of participants in the chat ||
|| **unreadId**  
[`integer\|null`](../../data-types.md) | ID of the first unread message ||
|| **lastMessageViews**  
[`string`](../../data-types.md) | JSON string with data on views of the last message ||
|| **markedId**  
[`integer\|null`](../../data-types.md) | ID of the marked message ||
|| **public**  
[`object\|string`](../../data-types.md) | Public access settings ||
|#

### Example Chat Object

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
    "textFieldEnabled": "Y",
    "backgroundId": null,
    "dateCreate": "2025-01-10T09:00:00+01:00",
    "lastMessageId": 789,
    "managerList": [1, 3],
    "messageCount": 42,
    "userCounter": 5,
    "unreadId": null
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
[`integer`](../../data-types.md) | ID of the author. `0` for system messages ||
|| **date**  
[`string\|null`](../../data-types.md) | Creation date in ISO 8601 format ||
|| **text**  
[`string`](../../data-types.md) | Text of the message. Maximum length — 20,000 characters. Longer messages are truncated with the suffix ` (...)` ||
|| **isSystem**  
[`boolean`](../../data-types.md) | Is the message a system message? ||
|| **uuid**  
[`string`](../../data-types.md) | UUID for deduplication ||
|| **forward**  
[`object\|null`](../../data-types.md) | Information about forwarding: `{id, userId, chatId, date}` or `null` ||
|| **params**  
[`object`](../../data-types.md) | Additional parameters: attach, keyboard, files, etc. ||
|| **viewedByOthers**  
[`boolean`](../../data-types.md) | Has the message been read by other participants? ||
|#

### Additional Fields (Only in Method Responses)

#|
|| **Field**  
`Type` | **Description** ||
|| **unread**  
[`boolean`](../../data-types.md) | Unread for the current user ||
|| **viewed**  
[`boolean`](../../data-types.md) | Viewed by the current user ||
|#

### Example Message Object

```json
{
    "id": 789,
    "chatId": 5,
    "authorId": 1,
    "date": "2025-01-15T10:30:00+01:00",
    "text": "Hello bot!",
    "isSystem": false,
    "uuid": "",
    "forward": null,
    "params": {},
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
[`string\|null`](../../data-types.md) | Upload date in ISO 8601 format ||
|| **type**  
[`string`](../../data-types.md) | Content type: `file`, `image`, `video`, `audio` ||
|| **name**  
[`string`](../../data-types.md) | File name with extension ||
|| **extension**  
[`string`](../../data-types.md) | File extension in lowercase ||
|| **size**  
[`integer`](../../data-types.md) | File size in bytes ||
|| **image**  
[`object\|false`](../../data-types.md) | Preview sizes for images: `{"height": 600, "width": 800}`, or `false` ||
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
    "image": false,
    "authorId": 1,
    "authorName": "John Smith",
    "isTranscribable": false,
    "isVideoNote": false,
    "isVoiceNote": false
}
```

## Command {#command}

Slash command of the bot. Returned in the responses from the methods [imbot.v2.Command.register](./imbot.v2/commands/command-register.md), [imbot.v2.Command.update](./imbot.v2/commands/command-update.md), [imbot.v2.Command.list](./imbot.v2/commands/command-list.md).

#|
|| **Field**  
`Type` | **Description** ||
|| **id**  
[`integer`](../../data-types.md) | Unique identifier of the command ||
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
|| **title**  
[`string`](../../data-types.md) | Title of the command in the account language. Only in method responses ||
|| **params**  
[`string`](../../data-types.md) | Description of the command parameters in the account language. Only in method responses ||
|| **category**  
[`string`](../../data-types.md) | Name of the bot that owns the command. Only in method responses ||
|| **context**  
[`string`](../../data-types.md) | Context of the command call. Only in method responses ||
|#

### Example Command Object

```json
{
    "id": 78,
    "botId": 456,
    "command": "/help",
    "common": false,
    "hidden": false,
    "extranetSupport": false,
    "title": "Show help",
    "params": "[query]",
    "category": "Support Bot",
    "context": "chat"
}
```

## Continue Learning

- [Chatbots 2.0: Overview of Methods](./index.md)
- [Bots: Overview of Methods](./imbot.v2/bots/index.md)
- [Event Formats for imbot.v2](./imbot.v2/events/events.md)
- [Event Formats im.v2](./im.v2/events/events.md)
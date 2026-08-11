# Files: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods allow you to upload files to the chat and receive a download link for the file on behalf of the user or application.

> Quick navigation: [all methods](#all-methods)

## How to Get Started {#how-to-start}

1. Upload the file with the [im.v2.File.upload](./file-upload.md) method. The method combines three steps of the previous API into a single call: it uploads the file to Drive, attaches it to the chat, and sends a message.
2. Retain `result.file.id` from the response — this is the file ID on Drive, and `result.messageId` — the ID of the created message with the file.
3. To obtain a download link, call [im.v2.File.download](./file-download.md), passing `dialogId` and this file ID in the `fileId` parameter.

The recipient is set by the `dialogId` parameter: for group chats — `chat{chatId}`, for private chats — the ID of the other participant. More details — [Format of dialogId](../../index.md#dialog-id). The description of the File object fields is available in [Objects and Fields](../../entities.md#file).

Both methods appeared only in `im.v2` and have no direct counterparts among the previous generation of messenger methods.

## Limits {#limits}

#|
|| **Limit** | **Value** ||
|| Maximum file size | 100 MB ||
|| Content transfer format | A Base64 string in `fields.content`, without the `data:*/*;base64,` prefix ||
|| Message text along with the file | The optional `fields.message` parameter ||
|#

How to prepare the file content — [How to Upload Files](../../../../files/how-to-upload-files.md).

## Relationship with Other Objects {#relations}

**User.** The methods run on behalf of the current user: the file appears in the chat as a file of this user, and registering a bot separately is not required.

**Chats.** A file is uploaded to a specific dialog. The chats themselves are created and configured with the methods of the [Chats in Bitrix24](../../../../chats/index.md) section.

**Events.** Uploading a file creates a message in the chat, so a subscribed application receives the [ONIMV2MESSAGEADD](../events/events.md#onimv2messageadd) event — [Events im.v2](../events/index.md).

**Files on behalf of a bot.** The same operations on behalf of a registered chatbot are performed by the [imbot.v2.File.upload and imbot.v2.File.download](../../imbot.v2/files/index.md) methods — they additionally require `botId`.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute the methods: a user or an application with access to the messenger

#| 
|| **Method** | **Description** ||
|| [im.v2.File.upload](./file-upload.md) | Uploads a file to the chat ||
|| [im.v2.File.download](./file-download.md) | Returns a link to download the file ||
|#

## Continue Your Exploration

- [API imbot.v2 Change Log](../../change-log.md)
- [im.v2: Overview of Methods](../index.md)
- [im.v2 Events](../events/index.md)
- [Files imbot.v2](../../imbot.v2/files/index.md)
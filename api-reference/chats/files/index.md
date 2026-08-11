# Files in Chats: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A file in a chat is a Drive object linked to a message. For new development, files are handled by the `im.v2.File.*` methods: a single call uploads the file, creates a message with it, and returns the file data. The subsection is part of the [Chats in Bitrix24](../index.md) section.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Chats in Bitrix24: Interface and Capabilities](https://helpdesk.bitrix24.com/open/21924784/)

## Which Methods to Choose

#|
|| **If You Need To** | **What to Use** ||
|| Upload a file to a chat | [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) ||
|| Get a link to download a file | [im.v2.File.download](../../chat-bots/chat-bots-v2/im.v2/files/file-download.md) ||
|| Save a file from a chat to your Drive | [im.disk.file.save](./im-disk-file-save.md) ||
|| Delete a file from the chat folder | [im.disk.file.delete](./im-disk-file-delete.md) ||
|| Maintain an integration built on a chain of Drive calls | [im.disk.folder.get](./im-disk-folder-get.md), [im.disk.file.commit](./im-disk-file-commit.md) ||
|| Work with files on behalf of a chatbot | [Files imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/files/index.md) ||
|#

## Order of Work with Files

1. Upload the file with the [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method
2. Retain `result.file.id` from the response — the file identifier, and `result.messageId` — the identifier of the created message
3. When you need to provide the file to the user again, get the link with the [im.v2.File.download](../../chat-bots/chat-bots-v2/im.v2/files/file-download.md) method — it needs both the file identifier and the `dialogId` of the chat

If the file is already in the chat and its identifier is unknown, retrieve the message history with the [im.dialog.messages.get](../messages/im-dialog-messages-get.md) method: it returns a `files` array where every file has an `id` field. This identifier is accepted by the download, save-to-Drive, and delete methods.

## Identifiers and Limits

- the current methods accept `dialogId`: `chat{chatId}` for a group chat, `{userId}` for a private dialog
- in the `im.disk.*` methods the identifiers are written in uppercase and differ from method to method — see the exact set in the parameters of the required method
- the file content is passed as a Base64 string — how to prepare it is described in the [How to Upload Files](../../files/how-to-upload-files.md) article
- the maximum file size is 100 MB

## Relationship with Other Objects

**Drive.** A chat file is stored on Drive: the `result.file.id` field is the identifier of the Drive object. The `im.v2.File.upload` method addresses Drive itself. In the old scenario, Drive was called manually: the chat folder was returned by the [im.disk.folder.get](./im-disk-folder-get.md) method, the file was uploaded by [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md), and the list of folder files — by [disk.folder.getchildren](../../disk/folder/disk-folder-get-children.md).

**Messages.** Uploading a file creates a message in the chat and returns its `messageId`, so a separate [im.message.add](../messages/im-message-add.md) call is not needed. From then on, this message is handled by the methods of the [Messages](../messages/index.md) subsection.

**Events.** A subscribed application learns about a new message with a file from the messenger events — they are collected in the [im.v2 Events](../../chat-bots/chat-bots-v2/im.v2/events/index.md) section.

**Chatbots.** The same operations on behalf of a bot are performed by the methods of the [Files imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/files/index.md) subsection.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the chat. Only the participant who sent the file can delete it

### Current Methods

#|
|| **Method** | **Description** ||
|| [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) | Uploads a file to the chat ||
|| [im.v2.File.download](../../chat-bots/chat-bots-v2/im.v2/files/file-download.md) | Returns a link for downloading the file ||
|#

### Methods with No Replacement in `im.v2` {#no-replacement}

These methods belong to the `im.disk.*` group, but they have no replacement in the new generation of the API yet — use them.

#|
|| **Method** | **Description** ||
|| [im.disk.file.save](./im-disk-file-save.md) | Saves a file from the chat to your Drive ||
|| [im.disk.file.delete](./im-disk-file-delete.md) | Deletes a file from the chat folder ||
|#

### Deprecated Methods {#legacy-methods}

These two methods are needed only by integrations already built on the old scenario. They are not recommended for new development.

#|
|| **Method** | **Description** | **Replacement in `im.v2`** ||
|| [im.disk.folder.get](./im-disk-folder-get.md) | Obtains the chat file storage folder | Not required: the upload method determines the folder itself ||
|| [im.disk.file.commit](./im-disk-file-commit.md) | Adds a file to the chat | [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) ||
|#

## Continue Learning

- [{#T}](../../chat-bots/chat-bots-v2/im.v2/index.md)
- [{#T}](../../chat-bots/chat-bots-v2/im.v2/files/index.md)
- [{#T}](../../chat-bots/chat-bots-v2/imbot.v2/files/index.md)
- [{#T}](../../files/index.md)
- [{#T}](../index.md)
# Files: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The methods upload files to a chat and return a download link for the uploaded file.

> Quick navigation: [all methods](#all-methods)

## How to Get Started {#how-to-start}

1. Upload the file with the [im.v2.File.upload](./file-upload.md) method. The method combines three steps of the previous API into a single call: it uploads the file to Drive, attaches it to the chat, and sends a message.
2. Retain `result.file.id` from the response — this is the file ID on Drive, and `result.messageId` — the ID of the created message with the file.
3. To retrieve a download link, call [im.v2.File.download](./file-download.md) and pass `result.file.id` in the `fileId` parameter.

The `dialogId` parameter of the [im.v2.File.upload](./file-upload.md) method specifies the chat to upload the file to: for group chats — `chat{chatId}`, for private chats — the ID of the other participant. More details — [Format of dialogId](../../index.md#dialog-id). The description of the File object fields is available in [Objects and Fields](../../entities.md#file).

Both methods were introduced in [im.v2](../index.md). In the previous generation, the same task was handled by a chain of three calls: [im.disk.folder.get](../../../../chats/files/im-disk-folder-get.md), [disk.folder.uploadFile](../../../../disk/folder/disk-folder-upload-file.md), and [im.disk.file.commit](../../../../chats/files/im-disk-file-commit.md). It is described in the [Files in Chats](../../../../chats/files/index.md) subsection.

## Limits {#limits}

#|
|| **Limit** | **Value** ||
|| Maximum file size | 100 MB ||
|| Content transfer format | A Base64 string in `fields.content`, without the `data:*/*;base64,` prefix ||
|| Message text along with the file | The optional `fields.message` parameter ||
|#

If the [im.v2.File.upload](./file-upload.md) method cannot accept a file, it returns an error, and the file does not appear in the chat. Typical errors:

- `FILE_TOO_LARGE` — the file exceeds the allowed size
- `FILE_INVALID_CONTENT` — the content is not in Base64 format or is passed with the `data:*/*;base64,` prefix
- `FILE_EMPTY` — the file name or its content is not passed

The error response structure is described in [Response Format](../../index.md#response-format), and the full list of codes is on the method page.

How to prepare the file content — [How to Upload Files](../../../../files/how-to-upload-files.md).

## Relationship with Other Objects {#relations}

**User.** The methods run on behalf of the user whose webhook or application OAuth token is passed in the request: the file appears in the chat as a file uploaded by that user, and registering a bot separately is not required. Authorization methods — [Authorization](../../index.md#auth).

**Chats.** A file is uploaded to a specific dialog. The chats themselves are created and configured with the methods of the [Chats in Bitrix24](../../../../chats/index.md) section.

**Events.** Uploading a file creates a message in the chat. If the application has subscribed the user to events with the [im.v2.Event.subscribe](../events/event-subscribe.md) method, it receives the [ONIMV2MESSAGEADD](../events/events.md#onimv2messageadd) event on the next call to [im.v2.Event.get](../events/event-get.md) — [im.v2 Events](../events/index.md).

**Files on behalf of a bot.** The same operations on behalf of a registered chatbot are performed by the [imbot.v2.File.upload and imbot.v2.File.download](../../imbot.v2/files/index.md) methods — they require `botId`, and when called via a webhook, also `botToken`.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the chat

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
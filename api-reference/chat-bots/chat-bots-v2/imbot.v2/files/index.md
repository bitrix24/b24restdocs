# Files: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods allow you to upload files to the chat on behalf of the bot and obtain a download link for the already uploaded file.

> Quick navigation: [all methods](#all-methods)

## How to Get Started {#how-to-start}

1. Upload the file with the [imbot.v2.File.upload](./file-upload.md) method. The method combines three steps of the previous API into a single call: it uploads the file to Drive, attaches it to the chat, and sends a message.
2. Retain `result.file.id` from the response — this is the file ID on Drive, and `result.messageId` — the ID of the created message with the file.
3. To provide the user or an external system with a link to the file, call [imbot.v2.File.download](./file-download.md), passing this ID in the `fileId` parameter.

A file is always uploaded to a specific dialog: the recipient is set by the `dialogId` parameter — [Format of dialogId](../../index.md#dialog-id). The description of the File object fields is available in [Objects and Fields](../../entities.md#file).

Both methods appeared only in `imbot.v2`. In the previous API, the same actions required three separate calls: uploading the file to Drive, attaching it to the chat, and sending a message. The full version mapping table is available in [Migration from imbot to imbot.v2](../../migration.md).

## Limits {#limits}

#|
|| **Limit** | **Value** ||
|| Maximum file size | 100 MB ||
|| Content transfer format | A Base64 string in `fields.content`, without the `data:*/*;base64,` prefix ||
|| Message text along with the file | The optional `fields.message` parameter ||
|#

How to prepare the file content — [How to Upload Files](../../../../files/how-to-upload-files.md).

## Relationship with Other Objects {#relations}

**Bot.** The file is uploaded on behalf of a registered bot: `botId` is passed in the calls, and for webhook authorization, `botToken` as well — [Bots](../bots/index.md).

**Messages.** Uploading a file creates a message in the chat, so there is no need to call [imbot.v2.Chat.Message.send](../messages/chat-message-send.md) separately. From that point on, you work with this message using the regular methods of the [Messages](../messages/index.md) group.

**Attachments.** If you need to present the file as a structured card rather than a regular message attachment, use the [file block in ATTACH](../messages/attachments/block-collections/files.md).

**Files on behalf of a user.** The same operations without registering a bot are performed by the [im.v2.File.upload and im.v2.File.download](../../im.v2/files/index.md) methods — they run on behalf of the current user and do not require `botId`.

## Overview of Methods {#all-methods}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the methods: owner of the registered bot

#| 
|| **Method** | **Description** ||
|| [imbot.v2.File.upload](./file-upload.md) | Uploads a file to the chat ||
|| [imbot.v2.File.download](./file-download.md) | Returns a link for downloading the file ||
|#

## Continue Your Exploration

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](../../index.md)
- [{#T}](../../entities.md)
- [{#T}](../../migration.md)
- [Messages imbot.v2](../messages/index.md)
- [{#T}](../messages/attachments/index.md)
- [Files im.v2](../../im.v2/files/index.md)
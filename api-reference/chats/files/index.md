# Files in Chats: Overview of Methods

Currently, the basic scenario for working with files in chats has become simpler: there is no need to separately obtain the chat folder, upload the file through Drive methods, and then link it to the message.

For new integrations, use the `im.v2` methods:

- [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) — uploads a file to the chat
- [im.v2.File.download](../../chat-bots/chat-bots-v2/im.v2/files/file-download.md) — returns a link to download the file

> Quick navigation: [new methods](#all-methods) | [legacy methods](#legacy-methods)

## How to Work with Files Now

Typical scenario:

1. Upload the file to the chat using [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md).
2. Use the obtained result in the application logic or chat interface.
3. When you need to provide the file to the user again, obtain the link via [im.v2.File.download](../../chat-bots/chat-bots-v2/im.v2/files/file-download.md).

{% note info "" %}

The `im.v2.File.*` methods belong to the new generation of the messenger API. They are recommended for use in new scenarios instead of the chain of calls to `im.disk.*` and Drive methods.

{% endnote %}

## What Has Changed Compared to the Old Scenario

Previously, uploading a file often required a chain of several actions:

- obtain the chat file storage folder
- upload the file via Drive methods
- link the uploaded file to the chat

Now, for most scenarios, direct methods are sufficient:

- [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md)
- [im.v2.File.download](../../chat-bots/chat-bots-v2/im.v2/files/file-download.md)

## New Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the methods: user or application with access to the messenger

#|
|| **Method** | **Description** ||
|| [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) | Uploads a file to the chat ||
|| [im.v2.File.download](../../chat-bots/chat-bots-v2/im.v2/files/file-download.md) | Returns a link for downloading the file ||
|#

## Legacy Methods {#legacy-methods}

If the integration is already built on the old scenario, you can use the `im.disk.*` methods:

#|
|| **Method** | **Description** ||
|| [im.disk.file.commit](./im-disk-file-commit.md) | Adds a file to the chat ||
|| [im.disk.file.save](./im-disk-file-save.md) | Saves a file to your Drive ||
|| [im.disk.file.delete](./im-disk-file-delete.md) | Deletes a file from the chat folder ||
|| [im.disk.folder.get](./im-disk-folder-get.md) | Obtains the chat file storage folder ||
|#

## Continue Learning

- [im.v2: Overview of Methods](../../chat-bots/chat-bots-v2/im.v2/index.md)
- [Files im.v2](../../chat-bots/chat-bots-v2/im.v2/files/index.md)
- [About Chats in Bitrix24](../index.md)
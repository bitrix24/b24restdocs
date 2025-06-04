# Files in Chats: Overview of Methods

To work with files, use the group of methods `im.disk.*`.

> Quick navigation: [all methods and events](#all-methods)

## Connection of Methods with Other Objects

**Chat.** Working with files is done using the chat identifier `CHAT_ID`. You can obtain the identifier by creating a chat with the method [im.chat.add](../im-chat-add.md) or retrieving the chat identifier with [im.chat.get](../im-chat-get.md).

**Drive Files.** The methods work with Drive files using the file identifier in the parameters `DISK_ID` or `UPLOAD_ID`. You can obtain the file identifier by retrieving the list of files with [disk.folder.getchildren](../../disk/folder/disk-folder-get-children.md) or by uploading a file with [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md).

## Add File to Chat

To add a file to a chat, use the method [im.disk.file.commit](./im-disk-file-commit). Specify the chat identifier `CHAT_ID` and one of the file identifier parameters:

-  `UPLOAD_ID` — temporary file identifier. It is assigned when the file is uploaded to the server using the [Drive module](../../disk/index.md).
-  `DISK_ID` — identifier of an existing file on the Drive.

You can add multiple files using an array, for example, `DISK_ID: [4837, 4839, 4841]`.

## Save File to Your Drive

The method [im.disk.file.save](./im-disk-file-save.md) allows you to save a file from the chat to your personal Drive in the Saved Files folder. If the folder does not exist, the system will create it.

## Delete File

When a file is no longer needed, it can be deleted from the chat folder using the method [im.disk.file.delete](./im-disk-file-delete). In the chat, the text *Message deleted* will be displayed instead of the file.

## Get Chat File Storage Folder

The method [im.disk.folder.get](./im-disk-folder-get) retrieves the identifier of the folder where chat files are stored. The identifier can be used in Drive methods, for example:
-  upload a file to the chat folder using the method [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md),
-  view the list of files in the folder using the method [disk.folder.getchildren](../../disk/folder/disk-folder-get-children.md).

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [im.disk.file.commit](./im-disk-file-commit.md) | Adds a file to the chat ||
|| [im.disk.file.save](./im-disk-file-save.md) | Saves a file to your Drive ||
|| [im.disk.file.delete](./im-disk-file-delete.md) | Deletes a file from the chat folder ||
|| [im.disk.folder.get](./im-disk-folder-get.md) | Retrieves the folder for storing chat files ||
|#
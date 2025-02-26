# About Chats in Bitrix24

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- from Sergey's file: individual, group, chat owner. where they appear, what scenarios are available for REST

{% endnote %}

{% endif %}

#| 
|| **Method** | **Description** ||
|| [im.chat.add](./im-chat-add.md) | Creates a chat ||
|| [im.chat.get](./im-chat-get.md) | Retrieves the chat ID ||
|| [im.counters.get](./im-counters-get.md) | Retrieves counters ||
|| [im.dialog.get](./im-dialog-get.md) | Retrieves chat data ||
|| [im.recent.get](./im-recent-get.md) | Retrieves a shortened list of recent chats ||
|| [im.recent.list](./im-recent-list.md) | Retrieves a list of chats ||
|#

## Changing a Chat

#| 
|| **Method** | **Description** ||
|| [im.chat.setOwner](./chat-update/im-chat-set-owner.md) | Changes the chat owner ||
|| [im.chat.updateAvatar](./chat-update/im-chat-update-avatar.md) | Changes the chat avatar ||
|| [im.chat.updateColor](./chat-update/im-chat-update-color.md) | Changes the chat color ||
|| [im.chat.updateTitle](./chat-update/im-chat-update-title.md) | Changes the chat title ||
|#

## Chat Participants

#| 
|| **Method** | **Description** ||
|| [im.chat.leave](./chat-users/im-chat-leave.md) | Leaves the chat ||
|| [im.chat.user.add](./chat-users/im-chat-user-add.md) | Invites participants to the chat ||
|| [im.chat.user.delete](./chat-users/im-chat-user-delete.md) | Removes participants from the chat ||
|| [im.chat.user.list](./chat-users/im-chat-user-list.md) | Retrieves chat participant IDs ||
|| [im.dialog.users.list](./chat-users/im-dialog-users-list.md) | Retrieves the list of participants ||
|#

## Working with Departments

#| 
|| **Method** | **Description** ||
|| [im.department.colleagues.list](./departments/im-department-colleagues-list.md) | Retrieves the list of users in your department ||
|| [im.department.employees.get](./departments/im-department-employees-get.md) | Retrieves the list of department employees ||
|| [im.department.get](./departments/im-department-get.md) | Retrieves information about the department ||
|| [im.department.managers.get](./departments/im-department-managers-get.md) | Retrieves the list of department managers ||
|#

## Working with Files in Chats

#| 
|| **Method** | **Description** ||
|| [im.disk.file.commit](./files/im-disk-file-commit.md) | Adds a file to the chat ||
|| [im.disk.file.delete](./files/im-disk-file-delete.md) | Deletes a file from the chat folder ||
|| [im.disk.file.save](./files/im-disk-file-save.md) | Saves a file to your disk ||
|| [im.disk.folder.get](./files/im-disk-folder-get.md) | Retrieves the file storage folder of the specified chat ||
|#

## Messages

#| 
|| **Method** | **Description** ||
|| [im.dialog.messages.get](./messages/im-dialog-messages-get.md) | Retrieves the list of recent messages ||
|| [im.dialog.read](./messages/im-dialog-read.md) | Marks messages as "read" ||
|| [im.dialog.unread](./messages/im-dialog-unread.md) | Marks messages as "unread" ||
|| [im.dialog.writing](./messages/im-dialog-writing.md) | Sends the "someone is typing..." status ||
|| [im.message.add](./messages/im-message-add.md) | Adds a message ||
|| [im.message.command](./messages/im-message-command.md) | Uses a bot command ||
|| [im.message.delete](./messages/im-message-delete.md) | Deletes a bot message ||
|| [im.message.like](./messages/im-message-like.md) | Changes the "Like" status of a message ||
|| [im.message.share](./messages/im-message-share.md) | Creates an object based on a message ||
|| [im.message.update](./messages/im-message-update.md) | Modifies a sent message ||
|#

## Notifications

#| 
|| **Method** | **Description** ||
|| [im.notify.answer](./notifications/im-notify-answer.md) | Responds to a notification that supports quick replies ||
|| [im.notify.confirm](./notifications/im-notify-confirm.md) | Interacts with notification buttons ||
|| [im.notify.delete](./notifications/im-notify-delete.md) | Deletes notifications ||
|| [im.notify.personal.add](./notifications/im-notify-personal-add.md) | Sends a personal notification ||
|| [im.notify.read.list](./notifications/im-notify-read-list.md) | Reads the list of notifications (except CONFIRM) ||
|| [im.notify.read](./notifications/im-notify-read.md) | Cancels read notifications ||
|| [im.notify.system.add](./notifications/im-notify-system-add.md) | Sends a system notification ||
|#

## Search

#| 
|| **Method** | **Description** ||
|| [im.search.chat.list](./search/im-search-chat-list.md) | Finds chats by names ||
|| [im.search.department.list](./search/im-search-department-list.md) | Finds departments ||
|| [im.search.last.add](./search/im-search-last-add.md) | Adds search to history ||
|| [im.search.last.delete](./search/im-search-last-delete.md) | Deletes search from history ||
|| [im.search.last.get](./search/im-search-last-get.md) | Retrieves search history ||
|| [im.search.user.list](./search/im-search-user-list.md) | Finds users ||
|#

## Special Operations in Chats

#| 
|| **Method** | **Description** ||
|| [im.chat.mute](./special-operations/im-chat-mute.md) | Disables notifications from the chat ||
|| [im.dialog.read.all](./special-operations/im-dialog-read-all.md) | Marks all chats as "read" ||
|| [im.recent.hide](./special-operations/im-recent-hide.md) | Removes a chat from the recent list ||
|| [im.recent.pin](./special-operations/im-recent-pin.md) | Pins a chat in favorites ||
|| [im.recent.unread](./special-operations/im-recent-unread.md) | Marks or unmarks a chat as "read" ||
|#

## Users

#| 
|| **Method** | **Description** ||
|| [im.user.get](./users/im-user-get.md) | Retrieves user data ||
|| [im.user.list.get](./users/im-user-list-get.md) | Retrieves user data ||
|| [im.user.status.get](./users/im-user-status-get.md) | Retrieves information about the user's current status ||
|| [im.user.status.idle.end](./users/im-user-status-idle-end.md) | Disables the automatic "Away" status ||
|| [im.user.status.idle.start](./users/im-user-status-idle-start.md) | Sets the automatic "Away" status ||
|| [im.user.status.set](./users/im-user-status-set.md) | Sets the user's status ||
|#
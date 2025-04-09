# Notifications in Chats: Overview of Methods

A chat notification is a message containing information from the system or a user. Notifications are managed by a group of methods im.notify.*.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [notifications in Bitrix24](https://helpdesk.bitrix24.com/open/19423886/)

## Connection of Notifications with Other Objects

**User.** A notification is sent to a user by the identifier `USER_ID`. You can obtain the user identifier using the method [user.get](../../user/user-get.md).

## Send Notification

You can send a message with information to the messenger, which will appear in the notification list.

- **Personal notification.** The employee will receive a notification from the user who initiated the method. A personal notification is sent by the method [im.notify.personal.add](./im-notify-personal-add.md).

- **System notification.** A notification from the system is sent by the method [im.notify.system.add](./im-notify-system-add.md).

Each notification can be associated with a unique tag `TAG`. If a new notification is sent with the same tag, the system will remove the previous notification.

Both methods return the notification identifier, which can be used in other methods.

## Read Notifications

The method [im.notify.read.list](./im-notify-read-list.md) manages the Read label for the list of notifications, except for the `CONFIRM` type with confirmation buttons. Using the `ACTION` parameter, you can mark notifications as read `Y` or unread `N`.

The method [im.notify.read](./im-notify-read.md) reads a single notification or all notifications after the specified one. The `ONLY_CURRENT` parameter defines the scope:

- `Y` — the Read label will be set only for the specified notification,

- `N` or absence of the parameter — for all notifications with `ID` equal to or greater than the specified one.

## Reply to Notification

You can reply to a notification that supports quick replies using the method [im.notify.answer](./im-notify-answer.md).

You can interact with buttons in the notification using the method [im.notify.confirm](./im-notify-confirm.md). The `NOTIFY_VALUE` parameter should contain the button value. For example, for a meeting invitation notification, the Accept button has a value of `Y` and the Decline button has a value of `N`.

## Delete Notification

The method [im.notify.delete](./im-notify-delete.md) deletes a notification by the identifier `ID` or by tags `TAG`, `SUB_TAG`.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** || 
|| [im.notify.personal.add](./im-notify-personal-add.md) | Sends a personal notification || 
|| [im.notify.system.add](./im-notify-system-add.md) | Sends a system notification || 
|| [im.notify.read.list](./im-notify-read-list.md) | Reads the list of notifications || 
|| [im.notify.read](./im-notify-read.md) | Reads a notification or all notifications from the specified one || 
|| [im.notify.answer](./im-notify-answer.md) | Replies to a notification that supports quick reply || 
|| [im.notify.confirm](./im-notify-confirm.md) | Interacts with notification buttons || 
|| [im.notify.delete](./im-notify-delete.md) | Deletes notifications || 
|#
# Attached File: Overview of Methods

The attached file links a document from the Drive to other Bitrix24 objects. This connection allows access to the file from various modules of the system.

> Quick Navigation: [all methods](#all-methods)

## Features of Working with Attached Files

The method `disk.attachedObject.get` operates on the record of attaching a file to a specific object, rather than the file itself. Imagine you have a file `report.docx` on the Drive. When you attach it to a task and a comment in the feed, two different attachment records are created with different identifiers, but both refer to the same file.

To obtain the connection identifier, you need to use methods that return attached files within specific objects. For example, when working with tasks, the connection identifier can be obtained through the method [tasks.task.get](../../tasks/tasks-task-get.md).

## Connection with Other Objects

**CRM.** Files can be attached to deals and estimates. The group of methods [crm.activity.*](../../crm/timeline/activities/index.md) is responsible for working with deals, while [crm.quote.*](../../crm/quote/index.md) handles estimates.

**Workflows.** You can initiate workflows for documents on the shared drive. Workflow management is performed using the methods [bizproc.workflow.*](../../bizproc/index.md).

**Tasks.** Files are attached to the task description. All participants in the task can view, edit, and download the attached files. You need to work with tasks through the methods of the group [tasks.task.*](../../tasks/index.md).

**Calendar.** Files are added to events and become accessible to all participants. You can obtain information about an event using the methods [calendar.event.*](../../calendar/index.md).

**News Feed.** Files are attached to messages and comments. The method [log.blogpost.get](../../log/log-blogpost-get.md) returns a list of messages, while the method [log.blogcomment.user.get](../../log/blogcomment/log-blogcomment-user-get.md) returns a list of comments.

**E-mail.** Attachments from e-mails are saved on the Drive. You can only work with e-mails through the Bitrix24 interface. If an e-mail is saved in CRM, you can interact with the attachments using the methods [crm.activity.*](../../crm/timeline/activities/index.md) through the `FILES` field.

**Universal Lists.** List items are linked to the Drive through the [field](../lists/fields/index) of type File (Drive). You can obtain the parameters of an item using the method [lists.element.get](../../lists/elements/lists-element-get.md).

**Chats.** Users can exchange documents, photos, videos, and audio. Files can be viewed and downloaded, and documents can be edited without downloading. Attached files are accessible to all chat participants. The method [im.dialog.messages.get](../../chats/messages/im-dialog-messages-get.md) returns a list of the latest messages.

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required file

#|
|| **Method** | **Description** ||
|| [disk.attachedObject.get](./disk-attached-object-get.md) | Returns information about the attached file ||
|#